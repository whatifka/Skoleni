# Tech Planning — Sprint 1 a architektura backendu

**Datum:** 18. března 2026
**Místo:** Zasedačka „Hrabal", Praha-Karlín
**Délka:** 9:00 – 10:30 (90 min, dodrženo)
**Zapsal:** Martin Doležal (Tech Lead)
**Status:** Schváleno

## Účastníci

- Martin Doležal (Tech Lead, backend)
- Jana Pokorná (iOS dev)
- Tomáš Veselý (Android dev)
- Tereza Marešová (PM)
- Ondřej Krejčí (QA)

## Cíl meetingu

1. Schválit cílovou architekturu backendu
2. Rozhodnout o autentizaci a SSO
3. Rozdělit Sprint 1 (30. 3. – 13. 4.)
4. Identifikovat technická rizika

## Klíčová rozhodnutí

### 1. Backend architektura: monolit s modulární strukturou

Po zvážení mikroservisů jsme se rozhodli pro **modulární monolit** v Node.js (TypeScript).

**Důvody:**
- Tým je malý (1 backend dev)
- E-shop už běží na Node.js monolitu — sdílíme infrastrukturu
- Mikroservisy by přidaly provozní složitost bez přínosu pro náš scope

**Struktura modulů:**
```
backend/
├── modules/
│   ├── auth/          # Přihlašování, JWT, OAuth
│   ├── catalog/       # Knihy, žánry, vyhledávání
│   ├── inventory/     # Skladová dostupnost (sync s NCR)
│   ├── orders/        # Objednávky online
│   ├── reservations/  # Rezervace v prodejnách
│   ├── loyalty/       # Věrnostní program, body
│   └── notifications/ # Push notifikace, e-maily
```

Detaily: viz [tech/architecture.md](../tech/architecture.md).

### 2. Autentizace: JWT s refresh tokeny + OAuth (Apple, Google)

- **Access token:** 15 minut, JWT (RS256)
- **Refresh token:** 30 dní, ukládá se v secure storage (Keychain / Keystore)
- **OAuth providers:** Apple Sign-In (povinné pro App Store), Google Sign-In
- **E-mail/heslo:** magic link (passwordless), žádná hesla v naší DB

**Důvod magic linku:** žádné heslo = žádné incidenty s úniky. Trade-off: closed first session experience, ale to už takhle dělá Notion, Slack, atd.

**Otevřená otázka OQ-2 z [design review](./2026-01-22-design-review.md) je tímto vyřešena: magic link.**

### 3. Synchronizace s NCR pokladnami: polling, ne webhooks

NCR pokladny **nemají webhooks** (zjistili jsme od Ládi z IT v lednu).

**Řešení:** backend pollne NCR REST API každých **60 sekund** pro:
- Stav skladu v každé prodejně
- Vyzvednuté rezervace
- Provedené nákupy s věrnostní kartou

**Cache:** Redis, TTL 60s pro skladovou dostupnost.

**Risk:** real-time přesnost dostupnosti +/- 60 sekund. V edge case (poslední kus v prodejně) může nastat „race condition", kdy app ukáže dostupné, ale mezitím se v prodejně prodá. **Mitigace:** rezervace má 2hodinové držení, takže pokladník při skenování QR uvidí, že kniha už není = informuje zákazníka. Není ideální, ale akceptovatelné pro MVP.

### 4. Push notifikace: Firebase Cloud Messaging (FCM)

Jednotná služba pro iOS i Android. APNS volá FCM transparentně.

**Typy notifikací v MVP:**
- Připravená rezervace
- Status objednávky (přijata, expedována, doručena)
- Akce v oblíbeném žánru (max 1x týdně)
- Připomenutí věrnostních bodů před expirací

### 5. CI/CD: GitHub Actions + Fastlane

- **Backend:** GitHub Actions → build → deploy na staging (každý merge do `main`) / produkci (manuálně, po smoke testech)
- **iOS:** Fastlane → TestFlight (każdý merge) → App Store (manuální release)
- **Android:** Fastlane → Internal Testing → Production track (postupný rollout 10% → 50% → 100%)

### 6. Monitoring a observability

- **APM:** Sentry (chyby ve všech 3 platformách)
- **Backend logs:** Pino → CloudWatch
- **Metriky:** Prometheus + Grafana (už máme pro e-shop)
- **Real User Monitoring na mobilu:** Sentry Performance

## Sprint 1 (30. 3. – 13. 4., 2 týdny)

### Backend (Martin)

- [ ] Setup repozitáře, CI, deploy na staging
- [ ] Modul `auth`: registrace, magic link, JWT, refresh
- [ ] Modul `catalog`: endpointy GET /books, GET /books/:id, GET /genres
- [ ] Database schema (Postgres) pro knihy, autory, žánry, uživatele
- [ ] Seed data: import katalogu z e-shopu (~45k titulů)

### iOS (Jana)

- [ ] Setup projektu, SwiftUI struktura
- [ ] Design system (barvy, typografie, komponenty z Figmy)
- [ ] Onboarding flow (3 obrazovky)
- [ ] Přihlášení (e-mail magic link + Apple Sign-In)

### Android (Tomáš)

- [ ] Setup projektu, Jetpack Compose struktura
- [ ] Design system (Material 3 token override podle brand)
- [ ] Onboarding flow
- [ ] Přihlášení (e-mail magic link + Google Sign-In)

### QA (Ondřej)

- [ ] Setup Maestro pro E2E testy na mobilu
- [ ] Test plan pro auth flow
- [ ] Manuální test plan pro Sprint 1 demo

## Technická rizika

| # | Risk | Severity | Mitigace |
|---|---|---|---|
| TR-1 | NCR API nestabilní / má rate limity, které neznáme | HIGH | Pilotní polling jen na 1 prodejně v Sprintu 2, monitoring |
| TR-2 | App Store review timeline (Apple) — nepředvídatelné | MEDIUM | První submission v 06/2026, ne 08/2026 |
| TR-3 | Magic link e-maily v spamu zákazníků | MEDIUM | Použijeme Postmark (vyšší doručitelnost než Mailgun) |
| TR-4 | Zaměstnanci v prodejnách neumějí používat skener QR | MEDIUM | Krátký fyzický leták + video tutorial, Lucie zajistí školení |
| TR-5 | Doporučovací engine — Jiří chce „aspoň jednoduchý" (z kickoff), tým ale nemá kapacity | LOW | Tereza ujednotí s Jiřím definici „jednoduchého" |

## Akční body

| # | Co | Kdo | Do kdy |
|---|---|---|---|
| 1 | Setup GitHub repo + branch protection | Martin | 24. 3. |
| 2 | Postmark účet a doménová autentizace (SPF/DKIM) | Martin | 26. 3. |
| 3 | NCR API dokumentace (vyžádat od Ládi) | Martin | 25. 3. |
| 4 | Apple Developer účet — verifikace | Jana | 27. 3. |
| 5 | Google Play účet — verifikace | Tomáš | 27. 3. |
| 6 | Maestro setup + první smoke test | Ondřej | 30. 3. |
| 7 | Diskuze s Jiřím o doporučovacím enginu | Tereza | 27. 3. |

## Otevřené otázky

- **Doporučovací engine v MVP:** stále nejasné, čeká na Terezu (viz TR-5)
- **Předplatné „Měsíc s knihou":** odložené, ale Pavel se ptá. Diskuze na příštím PM meetingu.
- **Tablet support:** Tereza chce, designerka to dělá, ale dev kapacita to zatím nepokrývá. **Doporučení Martina: NEdělat v MVP**, řešit v V1.1.

## Další tech meeting

**Datum:** 14. dubna 2026 (Sprint 1 retro + Sprint 2 planning)
