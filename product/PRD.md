# Product Requirements Document — Mobilní aplikace Knihy Agentovský

**Verze:** 1.3
**Status:** Schváleno pro implementaci (Sprint 1+)
**Autor:** Tereza Marešová
**Datum:** 1. prosince 2025, aktualizováno 15. března 2026

---

## 1. Shrnutí

Stavíme nativní mobilní aplikaci (iOS + Android) pro síť knihkupectví Knihy Agentovský. Aplikace sjednotí nákup online a v kamenných prodejnách, digitalizuje věrnostní program a umožní rezervaci knih v prodejně do 2 hodin.

**Cíl MVP:** vypustit do produkce do konce **Q3 2026** (původní deadline Q2 byl posunut po Sprint 1 retrospektivě).

## 2. Problém

Detailní analýza je v [business/kpi-prehled.md](../business/kpi-prehled.md). Stručně:

1. **Mobilní konverze e-shopu je 1,2 %** (vs. 3,4 % desktop). Roční ztráta ~28 mil. Kč.
2. **Věrnostní program** používá plastovou kartu, kterou má aktivních 31 % zákazníků.
3. **Konkurence (Martinus, Kosmas) má kvalitní app**, my nemáme nic.
4. **NPS klesá** dva roky (47 → 41), uživatelé si stěžují na mobilní web.

## 3. Cílová skupina

Plné personas v [personas.md](./personas.md). Tři klíčové segmenty:

- **Anna (38, učitelka, Praha):** vášnivá čtenářka, 30+ knih/rok, věrnostní zákaznice
- **Petr (45, IT manažer, Brno):** non-fiction, rád si rezervuje a vyzvedne v práci
- **Klára (27, copywriter, Olomouc):** kupuje dárky, hodně podle doporučení

## 4. Cíle a KPI

| Cíl | KPI | Cílová hodnota (12 měsíců od launche) |
|---|---|---|
| Zvýšit mobilní konverzi | Konverze v app | ≥ 3,0 % |
| Digitalizovat věrnostní program | Aktivní digitální karty | ≥ 60 % aktivních zákazníků |
| Snížit závislost na e-shopu | Podíl app na tržbách | ≥ 25 % |
| Zlepšit zákaznickou zkušenost | NPS app | ≥ 50 |
| Aktivace funkce rezervace | Měsíčních rezervací | ≥ 2000 |

## 5. Rozsah MVP

### 5.1 ZAHRNUTO v MVP (musí být)

| # | Funkce | Priorita | Detail |
|---|---|---|---|
| F-01 | Onboarding (3 obrazovky + výběr žánrů) | MUST | Skipovatelné |
| F-02 | Přihlášení (magic link, Apple, Google) | MUST | Bez hesla |
| F-03 | Vyhledávání knih (fulltext, filtry) | MUST | Žánr, autor, cena, dostupnost |
| F-04 | Detail knihy | MUST | Anotace, cena, dostupnost v prodejnách |
| F-05 | Košík a checkout | MUST | Apple Pay, Google Pay, karta |
| F-06 | Rezervace knihy v prodejně | MUST | 2hodinové držení, QR kód, platba na pokladně |
| F-07 | Digitální věrnostní karta | MUST | V app + Apple/Google Wallet |
| F-08 | Push notifikace | MUST | Status objednávky, rezervace |
| F-09 | Historie objednávek | MUST | Posledních 24 měsíců |
| F-10 | Profil a nastavení | MUST | Změna údajů, notifikační preference, GDPR |

### 5.2 ZAHRNUTO v MVP (mělo by být)

| # | Funkce | Priorita | Detail |
|---|---|---|---|
| F-11 | Wishlist | SHOULD | Dohodnuto v Sprint 2 retrospektivě (přidáno do scope) |
| F-12 | Jednoduchá doporučení („Bestsellery v žánru") | SHOULD | Žádné ML, jen pravidlové |
| F-13 | Sledování objednávky (tracking link) | SHOULD | Integrace s PPL, Zásilkovna |

### 5.3 NENÍ v MVP (vyloučeno, odložené na V1.1 nebo později)

| # | Funkce | Důvod odložení | Cílová verze |
|---|---|---|---|
| F-X1 | Doporučovací engine (ML-based) | Komplexita, žádná datová sada | V2 |
| F-X2 | Audio ukázky knih | Práva, velikost dat | V1.1 |
| F-X3 | Recenze od komunity | Moderace, právní rizika | V1.1 |
| F-X4 | Předplatné „Měsíc s knihou" | Jen odkaz na web v MVP | V1.2 |
| F-X5 | Tablet support | Dev kapacita | V1.1 |
| F-X6 | Sdílení knihy externě | Nice-to-have | V1.1 |
| F-X7 | Knihovní seznamy (booklists) | Komplexita UX | V2 |
| F-X8 | In-app chat se zákaznickou podporou | Personální nároky | V1.2 |

## 6. Klíčové user flows

Detailní flows jsou v [design/](../design/) (Figma). Hlavní:

1. **První spuštění:** Splash → Onboarding (3 obr.) → Domů (bez přihlášení)
2. **Vyhledání a nákup online:** Hledat → Detail → Do košíku → Checkout → Apple Pay → Potvrzení
3. **Rezervace v prodejně:** Detail knihy → „Rezervovat" → Vybrat prodejnu → Potvrdit → QR kód
4. **Vyzvednutí v prodejně:** Push notifikace „Připraveno" → Zobrazit QR → Pokladník skenuje → Platba → Hotovo
5. **Použití věrnostní karty:** Profil → Věrnostní karta → Zobrazit QR / Přidat do Wallet

## 7. Rozpočet a timeline

- **Rozpočet:** 4,8 mil. Kč (schváleno 12. 11. 2025)
- **Tým:** 1 PM, 1 designer (60 %), 3 vývojáři, 1 QA
- **Discovery + Design:** 11/2025 – 02/2026
- **Vývoj Sprint 1–10 (2 týdny každý):** 03/2026 – 07/2026
- **Beta testing:** 08/2026
- **Public launch:** 09/2026

⚠ Původní deadline byl Q2 2026 (požadavek CEO z kickoffu). Po vyhodnocení Sprintu 1 a realistických odhadů byl Tereza + Martin přesvědčili stakeholdery posunout na Q3 2026.

## 8. Závislosti

- **NCR pokladny:** integrace přes REST API (polling, 60s) — viz [tech-planning meeting](../meetings/2026-03-18-tech-planning.md)
- **Postmark:** doručování e-mailů (magic link)
- **Firebase Cloud Messaging:** push notifikace
- **Stripe:** platby (Apple Pay, Google Pay, karta)
- **Zásilkovna + PPL API:** tracking zásilek

## 9. Předpoklady a omezení

- Předpokládáme, že **NCR API zvládne polling z 14 prodejen každých 60s** bez rate limitu (TODO: ověřit v Sprintu 2)
- Předpokládáme, že **App Store review zabere max 3 týdny** v worst case (TODO: ověřit historií)
- **GDPR:** všechny data uživatelů v EU (Hetzner Nürnberg)
- **Jazyková podpora MVP:** pouze čeština. Slovenština ve V1.1.

## 10. Otevřené otázky

| # | Otázka | Vlastník | Status |
|---|---|---|---|
| OQ-1 | Jak zacházet s rezervací, když je kniha mezitím prodaná v prodejně? | Tereza + Lucie | Dohodnuto: zobrazit notifikaci, nabídnout jinou prodejnu (Sprint 4) |
| OQ-2 | Co s expirací věrnostních bodů? | Hana + Tereza | **Otevřeno** |
| OQ-3 | Bude v MVP zobrazení e-knih (digital downloads)? | Tereza + Jiří | **Otevřeno** — pravděpodobně ne |

## 11. Reference

- [Kickoff meeting](../meetings/2025-11-12-kickoff.md)
- [Design review](../meetings/2026-01-22-design-review.md)
- [Tech planning](../meetings/2026-03-18-tech-planning.md)
- [Design brief](../design/design-brief.md)
- [Personas](./personas.md)
- [Roadmap](./roadmap.md)
- [Risk register](../risks/risk-register.md)
