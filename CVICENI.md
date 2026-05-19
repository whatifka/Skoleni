# Cvičení: Práce s AI agentem nad reálným repozitářem

> **Pro studenty.** Cvičení trvá **60 minut**. Postupujte podle bloků níže.

## Než začneme

1. Naklonujte si tento repozitář lokálně:
   ```bash
   git clone git@github.com:whatifka/Skoleni.git
   cd Skoleni
   ```

2. Otevřete repozitář v **Claude Code** (nebo v jiném AI agentu — Cursor, Windsurf, Aider).

3. Krátce si prolistujte strukturu — **nečtěte detailně**, jen se zorientujte:
   ```
   business/    — finanční čísla
   meetings/    — 3 zápisy z porad
   product/     — PRD, personas, user stories, roadmap
   design/      — design brief
   tech/        — architektura + API
   research/    — konkurenční analýza
   risks/       — risk register
   ```

4. **Co je tento projekt:** fiktivní knihkupectví **Knihy Agentovský** staví novou mobilní aplikaci. Repozitář obsahuje produktovou a technickou dokumentaci.

---

## 🟢 Blok 1 (0–5 min) — Orientace

**Cíl:** zjistit, jak agent skládá kontext z více souborů.

**Úkol:** Zeptejte se agenta:

> *„O čem je tento projekt? Shrň mi to ve 3 odstavcích."*

**Sledujte:**
- Které soubory si agent přečetl?
- Vychází ze `README.md`? Z `CLAUDE.md`? Z `PRD.md`?
- Kolik souborů potřeboval, než dal smysluplnou odpověď?

**Otázka k zamyšlení:** Kdyby v repozitáři nebyl `CLAUDE.md`, dostali byste stejně dobrou odpověď?

---

## 🟢 Blok 2 (5–15 min) — Extrakce poznatků

**Cíl:** naučit se zadávat **specifické** úkoly, ne obecné.

**Úkol:** Zeptejte se agenta:

> *„Najdi a shrň všechna rozhodnutí, která padla ve třech meetinzích v adresáři `meetings/`. Vytvoř tabulku s těmito sloupci: rozhodnutí | meeting (datum) | kdo navrhl | status (otevřeno/uzavřeno)."*

**Co byste měli dostat:**
- Tabulku s cca **10–15 rozhodnutími**
- U některých rozhodnutí uvidíte, že **nejsou dořešená**
- U některých uvidíte **rozpory mezi meetingy**

**Bonus:** Porovnejte vaši odpověď se sousedem. Liší se? Proč?

**Lekce:** *„Shrň meetingy"* = vágní výstup. *„Extrahuj rozhodnutí do tabulky se sloupci X, Y, Z"* = použitelný výstup.

---

## 🟢 Blok 3 (15–30 min) — Generování artefaktu (ve dvojicích)

**Cíl:** vyzkoušet, jak AI syntetizuje napříč soubory.

Vyberte si **jednu** z variant níže:

### Varianta A — Rozšíření PRD

> *„Vytvoř draft PRD sekce pro funkci **'Audio ukázky knih'**, která má jít do V1.1. Použij stejnou strukturu, jakou má hlavní `product/PRD.md` (Shrnutí, Cílová skupina, Cíle a KPI, Rozsah, User flows, Závislosti, Rizika). Argumentuj, proč je tato funkce potřeba — vycházej z person a roadmapy."*

### Varianta B — Nové user stories

> *„Vytvoř 8 user stories pro epic **'Wishlist a sdílení'** (rozšíření Epic E7 v `product/user-stories.md`). Použij notaci Given/When/Then. Drž se priorit (P0/P1/P2) a velikostí (S/M/L/XL) podle stávajících stories. Stories navazují na potřeby persony **Klára**."*

### Varianta C — Onboarding pro zaměstnance

> *„Napiš markdown dokument: krok-po-kroku tutoriál pro **zaměstnance v kamenných prodejnách**, jak používat QR skener pro vyzvedávání rezervací. Cílová skupina: zaměstnanci 50+ let, **nízká technická zdatnost**. Tone of voice: vstřícný, trpělivý, žádné anglicismy. Maximálně 1 obrazovka A4."*

**Po dokončení:**
- Zreviewujte výstup. Co je výborné? Co je halucinace?
- Zkontrolujte: drží se agent **person** z `product/personas.md`?
- Drží se agent **tone of voice** z `design/design-brief.md`?

---

## 🟢 Blok 4 (30–45 min) — Analýza dat

**Cíl:** ověřit, jak agent pracuje s **CSV daty + kontextem**.

**Úkol:** Postupně se zeptejte:

1. > *„Otevři `business/financni-vykonnost.csv`. Jaký je trend tržeb, hrubé marže a EBITDA za posledních 5 let? Odpověz čísly."*

2. > *„Vypočti CAGR (Compound Annual Growth Rate) pro tržby mezi roky 2021 a 2025. Ukaž výpočet."*

3. > *„Vedení firmy schválilo 4,8 mil. Kč na mobilní app. **Dává to ekonomický smysl?** Argumentuj pomocí dat z CSV a kontextu z `business/kpi-prehled.md`. Pokud uvádíš čísla, vždy uveď zdroj."*

**Sledujte:**
- Spustil agent **kód** (Python), nebo jen odhadl?
- Jsou čísla **správně spočítaná**?
- U otázky 3: kombinuje agent CSV **s** vysvětlujícím textem v `kpi-prehled.md`?

**Lekce:** dobrý AI agent kombinuje **strukturovaná data** (CSV) s **kontextem** (markdown). Špatný jen čte jeden zdroj.

---

## 🟢 Blok 5 (45–55 min) — Kritické myšlení (skupiny po 3–4)

**Cíl:** najít chyby a rozpory, které lidi obvykle přehlédnou.

**Úkol:** Najděte v repozitáři **alespoň 3 nekonzistence nebo nedořešená rozhodnutí**.

Použijte k tomu agenta, ale **každý nález ověřte čtením souborů sami** (agent může vymyslet rozpor, který tam není).

**Co hledat:**
- Rozpory mezi tím, co padlo v různých meetinzích
- Otevřené otázky bez vlastníka nebo termínu
- Rozpory mezi PRD a roadmapou
- Funkce, které slibujeme uživatelům, ale není jasné, kdo je dodá
- Risky bez mitigace

**Příklad promtu, který vám pomůže:**

> *„Projdi všechny soubory v `meetings/`, `product/` a `risks/`. Najdi 5 míst, kde si dokumenty navzájem odporují, nebo kde je rozhodnutí otevřené bez termínu. U každého nálezu uveď přesný citát ze zdrojového souboru a název souboru."*

**Bonus pro skupinu:** kdo najde **rozpor, který ostatní skupiny nenašly**, dostane bod navíc.

---

## 🟢 Blok 6 (55–60 min) — Reflexe (společně)

Diskutujte ve skupině / s lektorem:

1. **Co agent zvládl výborně?**
2. **Kde agent halucinoval?** (Vymyslel si jména, čísla, citáty?)
3. **Která zadání dala nejlepší výstup?** Co měla společné?
4. **Co byste příště zadali jinak?**

**Klíčový takeaway:**

> *„AI agent je nejvýkonnější, když má **bohatý a strukturovaný kontext**. Soubory jako `CLAUDE.md`, `PRD.md` a meeting notes nejsou jen pro lidi — jsou i pro AI. Investice do dobré dokumentace se vrátí v rychlosti a kvalitě AI výstupů."*

---

## 🔥 Bonusové úkoly (když vám zbude čas)

### B1 — Konkurence
> *„Z `research/konkurence.md` vytáhni 3 největší příležitosti, kde můžeme předehnat Martinus. Argumentuj daty z `business/`."*

### B2 — Commit message
> *„Předstírej, že jsi autor PR, který přidává funkci 'Wishlist' (US-060 z user-stories.md). Napiš commit message a PR description ve formátu Conventional Commits."*

### B3 — Risk-based prioritizace
> *„Z `risks/risk-register.md` vyber 3 rizika s nejvyšším Score. Pro každé navrhni **další** mitigaci, která tam ještě není."*

### B4 — Backend scaffold
> *„Na základě `tech/api-spec.md` vygeneruj Express + TypeScript boilerplate pro endpoint `POST /reservations`. Použij Zod pro validaci. Drž se konvencí ze `tech/architecture.md`."*

### B5 — Push notifikace
> *„Napiš text push notifikace **'Připravena rezervace v prodejně Praha-Letná'**. Drž se tone of voice z `design/design-brief.md`. Vytvoř 3 varianty (krátká, střední, dlouhá)."*

---

## Po hodině — domácí úkol (volitelné)

Vyberte si **jednu funkci** ze sekce 5.3 v [product/PRD.md](product/PRD.md) (NENÍ v MVP, ale je v budoucích verzích) a vypracujte:

1. **PRD sekci** (struktura jako hlavní PRD)
2. **5 user stories** v Given/When/Then notaci
3. **Risk analysis** (3 rizika s Impact × Probability)

Použijte AI agenta na draft, ale **každou sekci osobně zreviewujte a opravte**. Odevzdejte spolu s krátkou reflexí: **co jste museli opravit a proč**.

---

## Tipy pro celou hodinu

✅ **Zadávejte specificky** — *„Najdi rozhodnutí ze 3 meetingů a udělej tabulku"* je 10x lepší než *„Shrň meetingy"*.

✅ **Ověřujte čísla** — agent může halucinovat. Pokud vám dá konkrétní číslo, najděte ho v souboru.

✅ **Přiložte konkrétní soubory** k promptu (`@cesta/k/souboru`), pokud chcete, aby agent koukal jen tam.

✅ **Když je výstup špatný, neopakujte stejný prompt** — přeformulujte ho. Specifičtěji. S příkladem.

✅ **Sledujte, do jakých souborů agent kouká** — pokud čte málo, dejte mu víc kontextu.

❌ **Nevěřte AI slepě.** Vždy zreviewujte. Reálný kód, který se nasazuje do produkce, **nikdy** neodevzdejte bez kontroly.
