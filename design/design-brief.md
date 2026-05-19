# Design Brief — Mobilní aplikace Knihy Agentovský

**Verze:** 1.2
**Datum:** 30. ledna 2026
**Autor:** Kateřina Bártová
**Schválila:** Tereza Marešová, Jiří Agentovský

---

## 1. Účel dokumentu

Tento brief definuje **vizuální a uživatelský směr** mobilní aplikace pro síť knihkupectví Knihy Agentovský. Slouží jako referenční dokument pro designery, vývojáře, marketing i copywritery.

## 2. Brand pozice

> **„Knihy Agentovský jsou knihkupectví, kde vás znají jménem. Aplikace má být digitálním pokračováním toho pocitu."**

Nejsme Amazon. Nejsme Martinus. Jsme **menší, osobnější, zkušenější**. Naše konkurenční výhoda je **kurátorství** (vybíráme knihy, nepředkládáme jich 50 000) a **lidský přístup** (znáte naše prodavače jménem, my chceme aby věděli, co rád/a čtete).

### Hodnoty značky

| Hodnota | Co to znamená v UI |
|---|---|
| **Důvěryhodnost** | Žádné agresivní notifikace, žádný „dark pattern" pro upselling. Cena vždy jasná. |
| **Kurátorství** | Doporučení jsou krátká a vysvětlená („Pokud máš rád/a Murakamiho, zkus..."). Žádné AI-generated bloat. |
| **Knihomilství** | Citáty z knih, typografie respektuje literaturu, animace jemné. |
| **Místní** | Prodejny jsou viditelné. „Vyzvedněte si do 2 hodin v Brně" je hero feature, ne fine print. |

### Hodnoty, kterými NEjsme

- ❌ Cool / hipster (jsme klasičtí)
- ❌ Levné / discountové (kvalita > množství)
- ❌ Korporátní (zůstáváme rodinní)
- ❌ Algoritmické (lidé > AI)

## 3. Cílová skupina

Plné personas jsou v [product/personas.md](../product/personas.md). Zkrácený přehled:

- **Primární:** ženy 30–55, města, vyšší vzdělání, čtou 12+ knih/rok
- **Sekundární:** muži 35–55, technicky orientovaní, čtou non-fiction
- **Terciární:** mladí dospělí 20–30, hledají dárky a osobní rozvoj

## 4. Tone of voice

### Princip: **Knihovník, ne barista, ne robot.**

- **Vykáme**, ale ne strojeně. „Doporučujeme vám tuto knihu" je OK. „Vážený zákazníku" je moc.
- **Krátké věty.** Aplikace není román.
- **Žádné anglicismy, kde jsou české alternativy.** „Wishlist" → „Seznam přání", „Checkout" → „Pokladna".
- **Humor jen kde sedí.** Empty state Knihovny: „Tady zatím nic není. To se brzy změní." OK. Push notifikace „Hej, koukni co máme!" NE.

### Příklady

| ❌ Nevhodné | ✅ Vhodné |
|---|---|
| „Bestseller alert! 🔥" | „Nová kniha týdne: Vesmír od Stuarta Cleryho" |
| „Login" | „Přihlásit se" |
| „Add to cart" | „Do košíku" |
| „Boost your reading game!" | „Knihy, které vás můžou bavit" |
| „You forgot something!" (košík) | „V košíku máte 2 knihy. Připomínáme." |
| „Buy now or regret later" | (tuto notifikaci nikdy neposíláme) |

## 5. Vizuální identita

### 5.1 Barvy

Schváleny v [design review](../meetings/2026-01-22-design-review.md).

**Primární paleta — „Teplá knihovna":**

| Token | Hex | Použití |
|---|---|---|
| `--color-bg-primary` | `#FAF6EE` | Pozadí (teplá béžová, ne čistě bílá) |
| `--color-bg-secondary` | `#F0E8D6` | Karty, sekundární pozadí |
| `--color-text-primary` | `#1F2A1D` | Hlavní text (tmavě zelená-černá) |
| `--color-text-secondary` | `#5C6757` | Sekundární text, popisky |
| `--color-accent` | `#2D5A3D` | CTA, odkazy, akcenty (tmavě zelená) |
| `--color-accent-hover` | `#1F4029` | Hover stav |
| `--color-warning` | `#B85C2A` | Varování (rezavá) |
| `--color-error` | `#A03C2A` | Chyby (tlumená červená, ne FF0000) |
| `--color-success` | `#3D7A4F` | Úspěch |

**Dark mode:**

| Token | Hex |
|---|---|
| `--color-bg-primary` | `#1A1E18` |
| `--color-bg-secondary` | `#252A22` |
| `--color-text-primary` | `#F0E8D6` |
| `--color-accent` | `#5FA876` |

### 5.2 Typografie

**Headings:** *Source Serif Pro* — serif, knižní pocit
**Body:** *Inter* — sans-serif, dobře čitelný na mobilu

| Token | Font | Size | Weight | Line height |
|---|---|---|---|---|
| `--type-display` | Source Serif Pro | 32px | 600 | 1.2 |
| `--type-h1` | Source Serif Pro | 24px | 600 | 1.25 |
| `--type-h2` | Source Serif Pro | 20px | 600 | 1.3 |
| `--type-body` | Inter | 16px | 400 | 1.5 |
| `--type-body-bold` | Inter | 16px | 600 | 1.5 |
| `--type-caption` | Inter | 13px | 400 | 1.4 |
| `--type-button` | Inter | 16px | 600 | 1.0 |

**Minimum font size:** 13px (caption). Pod to nejdeme, kvůli přístupnosti.

### 5.3 Spacing (8-point grid)

```
4, 8, 12, 16, 24, 32, 48, 64, 96
```

Standardní padding obrazovek: **16px** boční, **24px** vertikální mezi sekcemi.

### 5.4 Rohové rádia

- Karty: **12px**
- Tlačítka: **8px**
- Inputy: **8px**
- Obrázky knih: **4px** (jemné, aby vypadaly jako reálné knihy)

### 5.5 Ikony

- Library: **Lucide Icons** (open source, klean)
- Stroke width: 1.5px
- Velikost: 20px (inline), 24px (navigation), 32px (hero)

### 5.6 Obrázky a fotografie

- **Obálky knih:** vždy se zobrazují s mírným stínem (`box-shadow: 0 2px 8px rgba(0,0,0,0.12)`), aby vypadaly jako fyzické knihy.
- **Fotografie prodejen:** přirozené světlo, teplé tóny, lidé (ne prázdné regály).
- **Žádné stockové „business handshake" fotky.**

## 6. UX principy

### P1: Skenovatelnost > úplnost

Uživatel skroluje. Hierarchie informace musí být jasná na první pohled. Pokud něco není kritické, schovat pod „více informací".

### P2: Maximálně 2 tap na nákup

Z hlavní obrazovky musí být nákup hotov ve 2 tap (otevři detail knihy → koupit). Žádné mezikroky typu „přidat do košíku → zobrazit košík → checkout → zadat údaje". Apple Pay / Google Pay jsou default.

### P3: Off-line je důstojný stav

- Bez sítě uživatel vidí: oblíbené, historii, věrnostní kartu, naposledy prohlížené knihy.
- Žádný „No internet" prázdný screen.

### P4: Žádné modální dialogy pro důležitá rozhodnutí

Modal je OK pro potvrzení („Opravdu odstranit?"). Není OK pro souhlas s GDPR (full screen), nebo pro výběr platby (full screen).

### P5: Loading state vždy informativní

- Skeleton screen pro listy
- Spinner s textem („Načítám dostupnost v prodejnách...") pro operace > 1s
- Nikdy jen točící se kolečko bez kontextu

## 7. Přístupnost (a11y)

**Minimum: WCAG 2.1 AA.**

- Kontrast textu vs. pozadí: ≥ 4.5:1 (běžný text), ≥ 3:1 (velký text)
- Všechny interaktivní elementy: min. **44x44 pt** (Apple HIG) / **48dp** (Material)
- VoiceOver / TalkBack labels na všech ikonách a obrázcích
- Dynamic Type support na iOS (až 200 %)
- Dark mode jako equal citizen, ne afterthought
- Žádné informace předávané pouze barvou (např. „červená = chyba" — vždy + ikona + text)

## 8. Motion / animace

**Princip: Animace má účel, ne dekoraci.**

- Přechod mezi obrazovkami: 250ms, cubic-bezier(0.4, 0.0, 0.2, 1)
- Tap feedback: 100ms scale 0.97
- Loading skeletonů: shimmer effect 1.5s loop
- Push notifikace haptika: default systémová (žádné custom vibrace)

**Reduced motion (a11y):** všechny animace vypnuté, jen fade 100ms.

## 9. Komponenty (Design System)

Brand tokens jsou v Figma + jako JSON v `design/tokens.json` (TODO).

**Hlavní komponenty MVP:**

- `Button` (primary, secondary, ghost, destructive)
- `Input` (text, search, password)
- `Card` (kniha, prodejna, recenze)
- `Chip` (žánr, filtr)
- `Banner` (info, warning, success, error)
- `BottomSheet` (rezervace, výběr platby)
- `EmptyState` (s ilustrací a CTA)
- `LoadingSkeleton`

## 10. Co tento brief NEpokrývá

- Marketing materiály (web, sociální sítě) — řeší Pavel Novotný
- Brand guidelines kamenných prodejen — existuje samostatný dokument z 2019
- Email templates — řeší se v sprintu 4

## 11. Reference

- [meetings/2026-01-22-design-review.md](../meetings/2026-01-22-design-review.md) — schválené direction
- [product/personas.md](../product/personas.md) — pro koho designujeme
- [product/PRD.md](../product/PRD.md) — co aplikace dělá
