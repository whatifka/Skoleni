# Technická architektura

**Verze:** 1.1
**Schváleno:** [Tech planning, 18. 3. 2026](../meetings/2026-03-18-tech-planning.md)
**Vlastník:** Martin Doležal

---

## 1. Přehled

Aplikace má **3 hlavní vrstvy**:

```
┌─────────────────────────────────────────┐
│  Mobile clients (iOS, Android)          │
│  - SwiftUI (iOS), Jetpack Compose (And) │
│  - Sentry RUM, Offline cache (Room/CD)  │
└──────────────┬──────────────────────────┘
               │  HTTPS REST (JSON)
               │  JWT auth
               ▼
┌─────────────────────────────────────────┐
│  Backend — Node.js modulární monolit    │
│  - Express + TypeScript                 │
│  - Auth, Catalog, Inventory, Orders,    │
│    Reservations, Loyalty, Notifications │
└──────────────┬──────────────────────────┘
               │
       ┌───────┴────────┬────────┬─────────┐
       ▼                ▼        ▼         ▼
   ┌────────┐    ┌──────────┐ ┌─────┐ ┌──────────┐
   │ Postgres│    │  Redis   │ │ S3  │ │ External │
   │  (RDS)  │    │ (cache,  │ │     │ │ Services │
   │         │    │  queue)  │ │     │ │          │
   └────────┘    └──────────┘ └─────┘ └────┬─────┘
                                            │
                              ┌─────────────┼──────────────┐
                              ▼             ▼              ▼
                          NCR POS API   Postmark        Stripe
                          (polling)     (e-mail)        (platby)
                                                            │
                                                            ▼
                                                          FCM
                                                       (push)
```

## 2. Tech stack

### 2.1 Mobile

| Platforma | Jazyk | UI Framework | Stav management | HTTP klient |
|---|---|---|---|---|
| iOS (16+) | Swift 5.9 | SwiftUI | Observation framework | URLSession + async/await |
| Android (8+, API 26) | Kotlin 1.9 | Jetpack Compose | ViewModel + StateFlow | Ktor client |

**Sdílené principy:**
- Offline-first kde to dává smysl (oblíbené, věrnostní karta, historie)
- Sentry pro chyby + Performance monitoring
- Krátké, jednosměrné toky dat (Repository → UseCase → ViewModel → View)

### 2.2 Backend

| Vrstva | Technologie |
|---|---|
| Runtime | Node.js 20 LTS |
| Jazyk | TypeScript 5.3 |
| Framework | Express 4 (s middlewary, ne Nest — jednoduchost) |
| ORM | Prisma 5 |
| Validace | Zod |
| Logging | Pino → CloudWatch |
| Testování | Vitest + Supertest |

### 2.3 Datová vrstva

| Účel | Technologie |
|---|---|
| Hlavní DB | PostgreSQL 16 (AWS RDS, multi-AZ) |
| Cache + queue | Redis 7 (ElastiCache) |
| Object storage | AWS S3 (obálky knih, statika) |
| Vyhledávání | Postgres full-text search (fts) — pro MVP. Pokud nestačí, Meilisearch v V1.1 |

### 2.4 Infrastruktura

- **Cloud:** AWS, region `eu-central-1` (Frankfurt) kvůli GDPR
- **Container orchestration:** ECS Fargate (jednoduchost > Kubernetes)
- **CDN:** CloudFront pro obálky a statiku
- **DNS + WAF:** Cloudflare před AWS
- **CI/CD:** GitHub Actions
- **IaC:** Terraform

### 2.5 Externí služby

| Služba | Účel | Důvod volby |
|---|---|---|
| Stripe | Platby | Apple Pay, Google Pay, karta out-of-box |
| Postmark | Transakční e-maily | Vysoká doručitelnost magic linků |
| Firebase Cloud Messaging | Push notifikace | Jednotná pro iOS i Android |
| Sentry | Error tracking + RUM | Industry standard |
| PostHog | Product analytics | Self-hostable, GDPR friendly |

## 3. Backend — modulová struktura

```
backend/
├── src/
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.service.ts
│   │   │   ├── auth.repository.ts
│   │   │   ├── auth.dto.ts            # Zod schemas
│   │   │   └── auth.test.ts
│   │   ├── catalog/
│   │   ├── inventory/
│   │   ├── orders/
│   │   ├── reservations/
│   │   ├── loyalty/
│   │   └── notifications/
│   ├── shared/
│   │   ├── database/                  # Prisma client
│   │   ├── cache/                     # Redis wrapper
│   │   ├── queue/                     # BullMQ wrapper
│   │   ├── http/                      # Express setup, middleware
│   │   ├── observability/             # Sentry, Pino
│   │   └── errors/                    # Domain errors
│   ├── jobs/
│   │   ├── inventory-sync.job.ts      # Polling NCR
│   │   ├── notification-sender.job.ts
│   │   └── loyalty-expiry.job.ts
│   └── server.ts
├── prisma/
│   └── schema.prisma
└── tests/
    └── integration/
```

### 3.1 Pravidla modulů

- Moduly **nesmí** importovat z `internal` jiných modulů. Jen z `index.ts` (public API).
- Sdílené věci jdou do `shared/`.
- Každý modul má vlastní DB schema (Postgres schema), které jen on čte/píše.

## 4. Datový model (zjednodušený)

```
users
  id (uuid PK)
  email (unique)
  display_name
  created_at, updated_at

books
  id (uuid PK)
  isbn (unique)
  title, author, publisher, year, language
  price_cents
  cover_url
  description
  genre_ids (array)
  created_at, updated_at

inventory
  book_id (FK)
  store_id (FK)
  quantity
  last_synced_at
  PRIMARY KEY (book_id, store_id)

stores
  id (uuid PK)
  name, address, city, lat, lng
  opening_hours (jsonb)
  ncr_id (interní ID v pokladně)

orders
  id (uuid PK)
  user_id (FK)
  status (enum: pending, paid, shipped, delivered, cancelled)
  total_cents
  payment_method
  shipping_method
  shipping_address (jsonb)
  created_at, updated_at

order_items
  order_id (FK)
  book_id (FK)
  quantity
  unit_price_cents

reservations
  id (uuid PK)
  user_id (FK)
  book_id (FK)
  store_id (FK)
  status (enum: pending, picked_up, expired, cancelled)
  qr_token (unique)
  expires_at
  created_at

loyalty_accounts
  user_id (PK, FK)
  points
  card_number (unique)
  created_at, updated_at

loyalty_transactions
  id (uuid PK)
  user_id (FK)
  order_id (FK, nullable)
  points (signed integer)
  reason (enum: purchase, redemption, bonus, expiry)
  created_at
```

## 5. Klíčové scénáře

### 5.1 Magic link přihlášení

```
1. Client → POST /auth/magic-link { email }
2. Backend: vygeneruje krátkodobý token (15 min), uloží do Redis
3. Backend: pošle e-mail přes Postmark s odkazem na klienta (deep link)
4. User klikne na odkaz → Client otevře deep link s tokenem
5. Client → POST /auth/verify { token }
6. Backend: ověří, vrátí access + refresh token
7. Client uloží tokeny do Keychain / Keystore
```

### 5.2 Polling NCR pokladen

```
Background job (BullMQ) každých 60s:
1. Pro každou prodejnu pošle GET na NCR API /inventory?store_id=X
2. Diff s aktuálním stavem v Postgres
3. Update zasáhnutých řádků v inventory table
4. Invalidace Redis cache pro tyto knihy
5. Pokud kniha přešla z 0 → 1, trigger notifikaci uživatelům s wishlistem
```

### 5.3 Rezervace + vyzvednutí

```
Rezervace:
1. Client → POST /reservations { book_id, store_id }
2. Backend: zkontroluje dostupnost (lock row na inventory)
3. Sníží quantity o 1 (reserved), vytvoří reservation s expires_at = now + 2h
4. Vrátí qr_token (URL-safe random)
5. Backend pošle push notifikaci „Rezervace potvrzena"

Vyzvednutí:
1. Pokladník skenne QR kód
2. NCR POS POST /reservations/:qr_token/pickup (přes interní API gateway)
3. Backend označí jako picked_up, vytvoří order, přičte věrnostní body
4. Push „Děkujeme za nákup, získali jste X bodů"

Expirace:
- Cron každých 5 min: kontrola expirovaných rezervací
- Vrátí quantity zpět do inventory, status = expired
```

## 6. Bezpečnost

### Autentizace
- JWT RS256, asymetrické klíče v AWS KMS
- Access token: 15 min v paměti klienta (ne persisted)
- Refresh token: 30 dní v Keychain / Keystore (encrypted at rest)
- Magic linky: jednorázové, 15 min platnost

### Autorizace
- Role-based: `user` (defaultní), `admin` (pro budoucí backoffice)
- Per-resource ownership: `order.user_id === request.user.id`

### Ostatní
- HTTPS všude (HSTS preload)
- Rate limiting na auth endpointy (10 req/min/IP)
- CSP headers na webhooks
- Žádné PII v logách (Pino redact)
- Pravidelný security scan (npm audit + Snyk v CI)

## 7. Observability

| Co | Nástroj |
|---|---|
| Errors (mobile + backend) | Sentry |
| Logs (backend) | Pino → CloudWatch |
| Metriky (backend) | Prometheus + Grafana |
| RUM (mobile) | Sentry Performance |
| Product analytics | PostHog |
| Uptime monitoring | Better Uptime |

### Klíčové dashboards

1. **Health:** API latence p50/p95/p99, error rate, throughput
2. **Business:** denní objednávky, rezervace, registrace, NPS
3. **NCR sync:** úspěšnost polling, latence NCR API, počet diffů

## 8. Co NEděláme (a proč)

- **Mikroservisy** — small team, jeden monolit je rychlejší
- **GraphQL** — pro 1 klienta je REST dost; možná v budoucnu
- **Kubernetes** — ECS Fargate je 10x jednodušší pro náš scale
- **Vlastní recommendation engine** — žádná data; V2 nejdřív
- **Multi-region** — všichni naši zákazníci jsou v ČR
- **Server-side rendering** — nejsme web, jsme mobile

## 9. Otevřené otázky

- Stačí Postgres FTS pro ~45k titulů s fuzzy match? (TODO benchmark v Sprintu 2)
- Jaký je realistický rate limit NCR API? (TODO ověřit s Láďou)
- Jak řešit obrazové asset (obálky) — vlastní hosting v S3 vs. partnerské CDN nakladatelství?

## 10. Reference

- [tech/api-spec.md](./api-spec.md)
- [Tech planning meeting](../meetings/2026-03-18-tech-planning.md)
- [Risk register](../risks/risk-register.md)
