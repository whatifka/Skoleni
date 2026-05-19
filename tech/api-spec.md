# API Specification — Mobile Backend

**Verze:** v1
**Base URL (produkce):** `https://api.knihyagentovsky.cz/v1`
**Base URL (staging):** `https://api.staging.knihyagentovsky.cz/v1`
**Auth:** JWT Bearer token v `Authorization` headeru

---

## Konvence

- **Formát:** JSON, UTF-8
- **Datumy:** ISO 8601 (`2026-05-19T14:30:00Z`)
- **Peníze:** integer v haléřích (`price_cents`), nikdy decimal
- **Identifikátory:** UUID v4
- **Stránkování:** cursor-based, `?cursor=xxx&limit=20`
- **Lokalizace:** `Accept-Language: cs-CZ` (default `cs-CZ`)

### Error response (jednotný formát)

```json
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Kniha s ID 'abc-123' nebyla nalezena.",
    "details": null
  }
}
```

| HTTP | Code (interní) | Význam |
|---|---|---|
| 400 | `VALIDATION_ERROR` | Špatný formát requestu |
| 401 | `UNAUTHORIZED` | Chybí nebo neplatný token |
| 403 | `FORBIDDEN` | Token OK, ale uživatel nemá oprávnění |
| 404 | `RESOURCE_NOT_FOUND` | Zdroj neexistuje |
| 409 | `CONFLICT` | Konflikt (např. rezervace už neexistuje) |
| 429 | `RATE_LIMITED` | Příliš mnoho requestů |
| 500 | `INTERNAL_ERROR` | Server fault |

---

## 1. Auth

### POST /auth/magic-link
Pošle e-mail s magic link tokenem.

**Body:**
```json
{ "email": "user@example.com" }
```

**Response 200:**
```json
{ "ok": true, "expires_in_seconds": 900 }
```

### POST /auth/verify
Vymění magic link token za access + refresh token.

**Body:**
```json
{ "token": "magic-link-token-from-email" }
```

**Response 200:**
```json
{
  "access_token": "eyJhbGc...",
  "refresh_token": "rt_abc123...",
  "expires_in": 900,
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "display_name": "Anna P.",
    "created_at": "2026-01-15T10:00:00Z"
  }
}
```

### POST /auth/refresh
Vymění refresh token za nový access token.

**Body:**
```json
{ "refresh_token": "rt_abc123..." }
```

### POST /auth/oauth/apple
OAuth flow s Apple Sign-In.

**Body:**
```json
{ "identity_token": "...", "user": { "name": "Anna" } }
```

### POST /auth/oauth/google
Obdoba pro Google.

### POST /auth/logout
Invaliduje refresh token.

---

## 2. Catalog

### GET /books
Seznam knih s filtry.

**Query params:**
- `q` — fulltext (název, autor, ISBN)
- `genre_ids[]` — filtr žánrů
- `min_price`, `max_price` — v haléřích
- `in_stock_only` — bool
- `store_id` — dostupné v dané prodejně
- `sort` — `relevance` (default), `price_asc`, `price_desc`, `newest`
- `cursor`, `limit` (default 20, max 50)

**Response 200:**
```json
{
  "data": [
    {
      "id": "uuid",
      "isbn": "978-80-7432-123-4",
      "title": "Stoner",
      "author": "John Williams",
      "publisher": "Argo",
      "price_cents": 39900,
      "cover_url": "https://cdn.knihyagentovsky.cz/covers/uuid.jpg",
      "in_stock_online": true,
      "in_stock_stores_count": 3
    }
  ],
  "next_cursor": "eyJpZCI6Im..." 
}
```

### GET /books/:id
Detail knihy.

**Response 200:**
```json
{
  "id": "uuid",
  "isbn": "978-80-7432-123-4",
  "title": "Stoner",
  "author": "John Williams",
  "publisher": "Argo",
  "year": 2020,
  "language": "cs",
  "price_cents": 39900,
  "cover_url": "...",
  "description": "Tichý román o životě anglistického profesora...",
  "genres": [
    { "id": "uuid", "name": "Současná próza" }
  ],
  "page_count": 240,
  "availability": {
    "online": true,
    "stores": [
      {
        "store_id": "uuid",
        "store_name": "Knihy Agentovský Letná",
        "city": "Praha",
        "quantity": 3,
        "last_synced_at": "2026-05-19T14:29:45Z"
      }
    ]
  },
  "related_book_ids": ["uuid1", "uuid2", "uuid3"]
}
```

### GET /genres
Seznam žánrů.

---

## 3. Cart & Orders

### POST /cart/items
Přidá knihu do košíku.

### GET /cart
Aktuální košík.

### POST /orders
Vytvoří objednávku.

**Body:**
```json
{
  "items": [{ "book_id": "uuid", "quantity": 1 }],
  "shipping_method": "zasilkovna",
  "shipping_address": {
    "first_name": "Anna",
    "last_name": "Procházková",
    "street": "Letenská 5",
    "city": "Praha",
    "zip": "17000"
  },
  "payment_method": "apple_pay"
}
```

**Response 201:**
```json
{
  "id": "uuid",
  "status": "pending_payment",
  "total_cents": 41900,
  "stripe_payment_intent_client_secret": "pi_..."
}
```

### GET /orders
Historie objednávek (paginated).

### GET /orders/:id
Detail objednávky + tracking.

---

## 4. Reservations

### POST /reservations
Rezervuje knihu v prodejně.

**Body:**
```json
{ "book_id": "uuid", "store_id": "uuid" }
```

**Response 201:**
```json
{
  "id": "uuid",
  "status": "pending",
  "qr_token": "rsv_abc123def456",
  "qr_data_url": "data:image/png;base64,...",
  "expires_at": "2026-05-19T16:30:00Z",
  "store": { "id": "uuid", "name": "Knihy Agentovský Letná" },
  "book": { "id": "uuid", "title": "Stoner" }
}
```

**Možné errors:**
- 409 `OUT_OF_STOCK` — kniha mezitím prodaná
- 409 `RESERVATION_LIMIT_EXCEEDED` — uživatel má 10+ aktivních rezervací

### GET /reservations
Moje rezervace (pending + nedávno picked_up).

### DELETE /reservations/:id
Zruší rezervaci.

---

## 5. Loyalty

### GET /loyalty
Stav věrnostního účtu.

**Response 200:**
```json
{
  "points": 1240,
  "card_number": "KA000012345",
  "tier": "silver",
  "tier_progress": { "current": 1240, "next_tier_at": 2000 },
  "wallet": {
    "apple_pass_url": "https://api.knihyagentovsky.cz/v1/loyalty/wallet/apple",
    "google_pass_url": "https://api.knihyagentovsky.cz/v1/loyalty/wallet/google"
  }
}
```

### GET /loyalty/transactions
Historie transakcí.

### GET /loyalty/wallet/apple
Vrátí `.pkpass` soubor pro Apple Wallet.

### GET /loyalty/wallet/google
Redirect do Google Wallet.

---

## 6. User profile

### GET /me
Profil aktuálního uživatele.

### PATCH /me
Update profilu.

### GET /me/preferences
Preferované žánry, notifikační preference.

### PATCH /me/preferences

### POST /me/gdpr/export
Vytvoří async job na export dat. Vrátí job ID.

### GET /me/gdpr/export/:job_id
Stav job + download URL.

### DELETE /me
Anonymizuje účet (soft delete s 30denní lhůtou).

---

## 7. Stores

### GET /stores
Seznam prodejen (statická data).

### GET /stores/nearby
Prodejny podle GPS.

**Query:** `lat`, `lng`, `radius_km` (default 50)

---

## 8. Notifications

### POST /devices
Registrace zařízení pro push notifikace.

**Body:**
```json
{
  "fcm_token": "...",
  "platform": "ios",
  "app_version": "1.0.0",
  "os_version": "iOS 17.4"
}
```

### DELETE /devices/:token
Odhlášení (např. logout).

---

## 9. Webhooks (interní, nejde z aplikace)

### POST /webhooks/stripe
Stripe payment status updates.

### POST /webhooks/ncr/pickup
NCR pokladna oznamuje vyzvednutí rezervace.

**Headers:** `X-NCR-Signature: ...` (HMAC)

---

## Rate limits

| Endpoint | Limit |
|---|---|
| `POST /auth/*` | 10 / min / IP |
| `GET /books*` | 60 / min / user |
| `POST /reservations` | 10 / hod / user |
| Vše ostatní | 120 / min / user |

429 odpověď obsahuje `Retry-After` header.

---

## Versioning

- URL prefix `/v1`, `/v2`, ...
- Breaking changes = nová verze
- Stará verze min. 6 měsíců po deprecation announcement

## Reference

- [tech/architecture.md](./architecture.md)
- OpenAPI 3.0 spec: `tech/openapi.yaml` (TODO)
