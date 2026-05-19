# User Stories — MVP Backlog

Stories jsou organizované podle hlavních epiců. Každý story má:
- **ID**, **prioritu** (P0/P1/P2), **odhad** (S/M/L/XL)
- **Acceptance criteria** v notaci Given/When/Then
- **Personu**, pro kterou je primárně určená

---

## Epic E1: Onboarding a autentizace

### US-001 — Onboarding pro nového uživatele
**Jako** nový uživatel (Klára)
**chci** rychle pochopit, co aplikace umí
**abych** se rozhodla, jestli mě stojí za to si ji nechat.

- **Priorita:** P0 | **Odhad:** M
- **AC:**
  - Given: poprvé otevírám app
  - When: spustím ji
  - Then: vidím 3 obrazovky onboardingu (hodnota app)
  - And: můžu kdykoliv kliknout „Přeskočit" a jít rovnou do Domů
  - And: 3. obrazovka mi nabízí přihlášení (nebo skip)

### US-002 — Přihlášení magic linkem
**Jako** uživatelka (Anna)
**chci se** přihlásit bez hesla
**abych** si nemusela pamatovat další heslo.

- **Priorita:** P0 | **Odhad:** L
- **AC:**
  - Given: zadám e-mail
  - When: kliknu „Pošli mi přihlašovací odkaz"
  - Then: dostanu e-mail do 30 sekund
  - And: kliknutím na odkaz se přihlásím v app
  - And: jsem přihlášena po dobu 30 dní (refresh token)

### US-003 — Přihlášení přes Apple Sign-In
**Jako** uživatel iPhone (Petr)
**chci se** přihlásit jedním tapnutím přes Apple ID
**abych** nemusel zadávat e-mail.

- **Priorita:** P0 | **Odhad:** M
- **AC:**
  - Given: vidím přihlašovací obrazovku
  - When: tapnu „Pokračovat s Apple"
  - Then: provede se OAuth flow s Apple
  - And: jsem přihlášen
  - And: pokud jsem se přihlásil poprvé, dostanu možnost vyplnit jméno

### US-004 — Přihlášení přes Google
**Jako** uživatel Android (Klára)
**chci se** přihlásit přes Google
**abych** to měla rychlé.

- **Priorita:** P0 | **Odhad:** M
- **AC:** Obdobné jako US-003, pro Google.

### US-005 — Výběr oblíbených žánrů při onboardingu
**Jako** nový uživatel (Anna)
**chci** označit žánry, které mě zajímají
**abych** dostával/a relevantní doporučení.

- **Priorita:** P1 | **Odhad:** S
- **AC:**
  - Given: jsem v onboardingu na obrazovce „Co tě baví"
  - When: vyberu 1+ žánrů (chip tlačítka)
  - Then: výběr se uloží do mého profilu
  - And: na Domů uvidím sekci „Pro tebe" s knihami z těchto žánrů

---

## Epic E2: Vyhledávání a katalog

### US-010 — Vyhledávání knihy fulltextem
**Jako** uživatel (Petr)
**chci** najít knihu podle názvu nebo autora
**abych** k ní rychle došel.

- **Priorita:** P0 | **Odhad:** L
- **AC:**
  - Given: jsem v sekci Hledat
  - When: napíšu min. 2 znaky
  - Then: vidím výsledky do 500ms
  - And: výsledky obsahují název, autora, obálku, cenu
  - And: pokud nic nenajdu, vidím „Nic jsme nenašli" + návrh „Možná jste mysleli..."

### US-011 — Vyhledávání s tolerancí překlepů
**Jako** uživatel (Petr)
**chci**, aby app pochopila i překlep
**abych** dostal výsledek, i když napíšu „murakahmi" místo „Murakami".

- **Priorita:** P1 | **Odhad:** M
- **AC:**
  - Given: hledám výraz s překlepem (1–2 znaky)
  - When: nic přesného nenajdu
  - Then: vidím fuzzy match výsledky pod nadpisem „Možná jste mysleli..."

### US-012 — Filtrování v katalogu
**Jako** uživatelka (Anna)
**chci** filtrovat knihy podle žánru, ceny a dostupnosti
**abych** našla, co hledám.

- **Priorita:** P1 | **Odhad:** M
- **AC:**
  - Given: jsem v katalogu nebo výsledcích vyhledávání
  - When: otevřu filtr
  - Then: můžu vybrat 1+ žánrů, cenové rozpětí, jen skladem
  - And: aplikace filtru je instantní (bez „Aplikovat" tlačítka, pokud možno)

### US-013 — Detail knihy s anotací
**Jako** uživatel (všechny persony)
**chci** vidět anotaci, autora, cenu, dostupnost
**abych** se rozhodl/a pro nákup.

- **Priorita:** P0 | **Odhad:** L
- **AC:** Viz [design brief](../design/design-brief.md) sekce Detail knihy.

### US-014 — Dostupnost v prodejnách (real-time)
**Jako** uživatelka (Anna)
**chci** vidět, ve kterých prodejnách je kniha skladem
**abych** věděla, kam jít.

- **Priorita:** P0 | **Odhad:** XL
- **AC:**
  - Given: jsem na detailu knihy
  - When: rozkliknu „Dostupnost v prodejnách"
  - Then: vidím seznam prodejen seřazený podle vzdálenosti (pokud mám location permission)
  - And: u každé prodejny je status: „Skladem (3 ks)" / „Poslední kus" / „Vyprodáno"
  - And: data nejsou starší než 60 sekund

---

## Epic E3: Nákup online

### US-020 — Přidat knihu do košíku
**Jako** uživatel (Petr)
**chci** přidat knihu do košíku
**abych** ji mohl koupit.

- **Priorita:** P0 | **Odhad:** S

### US-021 — Checkout s Apple Pay
**Jako** uživatel iPhone (Petr)
**chci** zaplatit Apple Pay
**abych** nemusel zadávat kartu.

- **Priorita:** P0 | **Odhad:** L

### US-022 — Checkout s Google Pay
**Jako** uživatel Android (Klára)
**chci** zaplatit Google Pay.

- **Priorita:** P0 | **Odhad:** L

### US-023 — Platba kartou (fallback)
**Jako** uživatel bez mobilní peněženky
**chci** zaplatit kartou
**abych** mohl/a koupit.

- **Priorita:** P0 | **Odhad:** M

### US-024 — Výběr způsobu doručení
**Jako** uživatel (Petr)
**chci** vybrat doručení na adresu nebo na výdejní místo
**abych** si mohl/a zvolit, co mi vyhovuje.

- **Priorita:** P0 | **Odhad:** M
- **AC:**
  - Možnosti: domů (PPL), Zásilkovna, výdejní box (PPL Parcel Shop)
  - Cena dopravy a odhadovaný termín se zobrazí u každé možnosti

---

## Epic E4: Rezervace v prodejně

### US-030 — Rezervovat knihu v prodejně
**Jako** uživatel (Petr)
**chci** rezervovat knihu v konkrétní prodejně
**abych** si ji mohl cestou z práce vyzvednout.

- **Priorita:** P0 | **Odhad:** XL
- **AC:**
  - Given: jsem na detailu knihy, která je skladem v 1+ prodejnách
  - When: tapnu „Rezervovat v prodejně"
  - Then: vyberu prodejnu (seznam autosorted podle vzdálenosti)
  - And: potvrdím rezervaci
  - And: dostanu push notifikaci „Rezervace potvrzena, vyzvedněte do [čas + 2h]"
  - And: v sekci „Knihovna → Rezervace" vidím QR kód

### US-031 — Vyzvednutí rezervace v prodejně
**Jako** uživatel
**chci** vyzvednout rezervaci pomocí QR kódu
**abych** nemusel/a říkat své jméno.

- **Priorita:** P0 | **Odhad:** M
- **AC:**
  - Given: jsem v prodejně s rezervací
  - When: ukážu QR kód pokladníkovi
  - Then: pokladník skenne a vidí mou rezervaci v NCR systému
  - And: provedu platbu na pokladně
  - And: rezervace zmizí z mé app, objeví se v historii

### US-032 — Notifikace o blížící se expiraci rezervace
**Jako** uživatel
**chci** být upozorněn 30 min před vypršením
**abych** to nezapomněl/a vyzvednout.

- **Priorita:** P1 | **Odhad:** S

---

## Epic E5: Věrnostní program

### US-040 — Digitální věrnostní karta v app
**Jako** uživatelka (Anna)
**chci** mít věrnostní kartu v telefonu
**abych** ji nemusela nosit v peněžence.

- **Priorita:** P0 | **Odhad:** M

### US-041 — Přidání karty do Apple Wallet
**Jako** uživatel iPhone
**chci** přidat věrnostní kartu do Wallet
**abych** ji měl/a po ruce na zamykací obrazovce.

- **Priorita:** P1 | **Odhad:** L

### US-042 — Přidání karty do Google Wallet
**Jako** uživatel Android
**chci** přidat věrnostní kartu do Google Wallet.

- **Priorita:** P1 | **Odhad:** L

### US-043 — Zobrazení stavu věrnostních bodů
**Jako** uživatelka (Anna)
**chci** vidět, kolik mám bodů a co za ně dostanu
**abych** se mohla rozhodnout, kdy je uplatnit.

- **Priorita:** P0 | **Odhad:** M

### US-044 — Automatické přičtení bodů po nákupu
**Jako** uživatel
**chci**, aby se body přičetly automaticky
**abych** si nemusel/a o nic žádat.

- **Priorita:** P0 | **Odhad:** L
- **AC:**
  - Body se přičtou do 5 minut po nákupu (online i v prodejně)
  - Push notifikace „Získali jste X bodů za nákup"

---

## Epic E6: Profil a historie

### US-050 — Historie objednávek
**Jako** uživatel (Petr)
**chci** vidět seznam svých objednávek
**abych** věděl, co jsem už kupoval.

- **Priorita:** P0 | **Odhad:** M

### US-051 — Detail objednávky a sledování
**Jako** uživatel
**chci** vidět status objednávky včetně tracking linku
**abych** věděl, kdy přijde.

- **Priorita:** P1 | **Odhad:** L

### US-052 — Editace osobních údajů
**Jako** uživatel
**chci** upravit jméno, adresu, telefon
**abych** měl/a aktuální data.

- **Priorita:** P0 | **Odhad:** S

### US-053 — Nastavení notifikací
**Jako** uživatel
**chci** vypnout marketingové notifikace
**abych** dostával/a jen ty důležité.

- **Priorita:** P0 | **Odhad:** M

### US-054 — GDPR — export a smazání dat
**Jako** uživatel
**chci** stáhnout svoje data a smazat účet
**abych** měl/a kontrolu nad svými údaji.

- **Priorita:** P0 | **Odhad:** L

---

## Epic E7: Wishlist (přidáno v Sprint 2 retro)

### US-060 — Přidání knihy do wishlistu
**Jako** uživatelka (Klára)
**chci** uložit knihu k pozdějšímu nákupu
**abych** se k ní mohla vrátit.

- **Priorita:** P1 | **Odhad:** S

### US-061 — Sdílení wishlistu
**Jako** uživatelka (Klára)
**chci** poslat svůj wishlist kamarádce
**aby** věděla, co mi koupit k narozeninám.

- **Priorita:** P2 | **Odhad:** M

---

## Souhrn priorit

| Priorita | Počet stories | Vysvětlení |
|---|---|---|
| **P0** | 19 | Bez nich není MVP. Musí být v Q3 2026 launchi. |
| **P1** | 8 | Mělo by být, řeší se postupně sprint po sprintu. |
| **P2** | 1 | Nice-to-have, pokud zbude čas. |

**Velikost backlogu MVP:** 28 stories
**Celkem odhad:** ~85 story points (S=1, M=3, L=5, XL=8)

## Co NENÍ v backlogu (vyloučeno z MVP)

Viz [PRD.md sekce 5.3](./PRD.md#53-není-v-mvp).
