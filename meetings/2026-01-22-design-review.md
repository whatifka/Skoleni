# Design Review #1 — Mobilní aplikace

**Datum:** 22. ledna 2026
**Místo:** Online (Google Meet)
**Délka:** 10:00 – 11:30 (90 min, dodrženo)
**Zapsala:** Kateřina Bártová (Design)
**Status:** Schváleno s drobnými připomínkami

## Účastníci

- Tereza Marešová (PM)
- Kateřina Bártová (Design)
- Martin Doležal (Tech Lead)
- Jana Pokorná (iOS dev)
- Hana Agentovská (CFO, prvních 30 min)

## Cíl meetingu

Schválit:
1. Information architecture (IA) aplikace
2. Hlavní user flows (onboarding, vyhledávání, rezervace, věrnostní program)
3. Vizuální direction (color, typography, ikonografie)

Materiály k revizi byly rozeslány 19. 1. (Figma odkaz v interním Notion).

## Klíčová rozhodnutí

### 1. Information Architecture: 4 hlavní sekce v bottom navigation

Schválena struktura:

| Sekce | Ikona | Účel |
|---|---|---|
| **Domů** | dům | Personalizovaný feed, doporučení, akce |
| **Hledat** | lupa | Vyhledávání + procházení žánrů |
| **Knihovna** | knihy | Můj wishlist, historie, věrnostní karta |
| **Účet** | osoba | Profil, objednávky, nastavení |

**Zamítnuto:** 5 sekcí včetně „Prodejny". Místo toho budou prodejny dostupné z detailu knihy a z menu.

**Poznámka:** Tereza navrhla pojmenovat „Knihovna" jako „Moje", aby bylo jasnější, že jde o uživatelská data. Hlasování: 4:1 pro „Knihovna" (termín z brand briefu).

### 2. Onboarding: 3 obrazovky max, skipovatelné

- **Obrazovka 1:** Hodnota app (rezervace, doporučení, věrnost)
- **Obrazovka 2:** Výběr oblíbených žánrů (5–10 chip tlačítek)
- **Obrazovka 3:** Přihlášení / registrace (e-mail, Apple, Google) nebo „Přeskočit"

Skip povoluje použití app bez účtu pro browsing. Pro nákup je přihlášení vyžadováno až v checkoutu.

### 3. Detail knihy: nejdůležitější obrazovka

Shoda na hierarchii:
1. Obálka + název + autor
2. Cena + tlačítka „Do košíku" / „Rezervovat v prodejně"
3. Hvězdičky + počet recenzí (zatím externí, např. Goodreads API — nebude v MVP)
4. Anotace (collapsible po 4 řádcích)
5. Dostupnost v prodejnách (real-time, expandable list)
6. Související tituly (max 6)

**Akční bod:** Kateřina dodělá responsive variantu pro tablet do 5. 2.

### 4. Rezervace v prodejně: 3-krokový flow

1. Vybrat prodejnu (autosort podle vzdálenosti, fallback podle posledních návštěv)
2. Potvrdit (max doba držení 2 hodiny)
3. Potvrzení + QR kód pro vyzvednutí na pokladně

Bez platby — platba na pokladně při vyzvednutí. *(Důvod: zaměstnanci v prodejnách to ulehčí — jen oskenuje QR, normální nákup.)*

### 5. Věrnostní karta: digitální karta v aplikaci + Apple/Google Wallet

- V app: záložka v Knihovně
- Apple Wallet / Google Wallet: tlačítko „Přidat do Wallet" v profilu
- Body se počítají při skenování QR kódu na pokladně

### 6. Vizuální direction: schválen návrh A („Teplá knihovna")

Tři návrhy byly připraveny:
- **A — Teplá knihovna:** teplá béžová, tmavě zelený akcent, serif pro názvy
- **B — Moderní minimal:** bílá, černá, oranžový akcent, sans-serif
- **C — Knižní klub:** tmavá tmavomodrá, krémová, mix sans+serif

**Hlasování:** A (4 hlasy), C (1 hlas — Martin), B (0).

**Komentář Hany:** „Vypadá to drahé. A draho. Jako bookshop, ne jako Amazon. Líbí se mi."

**Akční bod:** Kateřina připraví finální brand tokens (color, typography, spacing) do 30. 1.

## Otevřené otázky (k vyřešení do 5. 2.)

| # | Otázka | Vlastník |
|---|---|---|
| OQ-1 | Jak bude vypadat „prázdný stav" Knihovny pro nového uživatele? | Kateřina |
| OQ-2 | Validace e-mailu — link nebo OTP kód? | Martin (tech feasibility) + Tereza |
| OQ-3 | Mají se v detailu knihy zobrazovat audio ukázky? *(Bylo v kickoffu vyloučeno z MVP, ale UX bez nich vypadá chudě)* | Tereza diskutuje s Jiřím |
| OQ-4 | Co dělá tlačítko „Rezervovat", když není kniha skladem nikde? | Kateřina + Tereza |

## Akční body

| # | Co | Kdo | Do kdy |
|---|---|---|---|
| 1 | Finální brand tokens (Figma + JSON pro vývojáře) | Kateřina | 30. 1. |
| 2 | Responsive varianta detail knihy (tablet) | Kateřina | 5. 2. |
| 3 | Tech feasibility check pro Apple/Google Wallet | Martin | 28. 1. |
| 4 | Diskuze s Jiřím o audio ukázkách v MVP | Tereza | 29. 1. |
| 5 | High-fidelity prototyp 5 hlavních flows | Kateřina | 12. 2. |
| 6 | Usability testing prototypu (5 uživatelů) | Tereza + Kateřina | do 26. 2. |

## Schváleno

- IA (4 sekce)
- Onboarding flow
- Detail knihy hierarchy
- Rezervační flow
- Věrnostní karta včetně Wallet integrace
- Vizuální direction A

## Další design review

**Datum:** 26. února 2026, 10:00
**Téma:** Výsledky usability testingu + finální high-fidelity návrhy
