# Cvičení: Práce s AI agentem nad reálným repozitářem

> **Pro studenty.** Cvičení trvá **60 minut**. Postupujte podle bloků níže.

## Než začneme

1. Naklonujte si tento repozitář lokálně:
   ```https://github.com/whatifka/Skoleni
   ```

2. Otevřete repozitář ve VS Code.

3. Krátce si prolistujte strukturu — **nečtěte detailně**, jen se zorientujte:
   ```
   business/    — finanční čísla a firemní ukazatele
   meetings/    — 3 zápisy z porad
   product/     — popis aplikace, cílových uživatelů a plánu funkcí
   design/      — vzhled a styl komunikace
   tech/        — technická architektura a popis API
   research/    — konkurenční analýza
   risks/       — seznam rizik
   ```

4. **Co je tento projekt:** fiktivní knihkupectví **Knihy Agentovský** staví novou mobilní aplikaci. Repozitář obsahuje produktovou a technickou dokumentaci.

---

## 🟢 Blok 1 (5 min) — Orientace

**Cíl:** zjistit, jak agent skládá kontext z více souborů.

**Úkol:** Zeptejte se agenta:

> *„O čem je tento projekt? Shrň mi to ve 3 odstavcích."*

**Sledujte:**
- Které soubory si agent přečetl?
- Vychází ze `README.md`? Z `CLAUDE.md`? Z `PRD.md`?
- Kolik souborů potřeboval, než dal smysluplnou odpověď?

**Otázka k zamyšlení:** Kdyby v repozitáři nebyl `CLAUDE.md`, dostali byste stejně dobrou odpověď?

---

## 🟢 Blok 2 (10 min) — Extrakce poznatků

**Cíl:** naučit se zadávat **specifické** úkoly, ne obecné.

**Úkol:** Zeptejte se agenta:

> *„Najdi a shrň všechna rozhodnutí, která padla ve třech meetinzích v adresáři `meetings/`. Vytvoř tabulku s těmito sloupci: rozhodnutí | meeting (datum) | kdo navrhl | status (otevřeno/uzavřeno)."*

**Co byste měli dostat:**
- Tabulku s cca **10–15 rozhodnutími**
- U některých rozhodnutí uvidíte, že **nejsou dořešená**
- U některých uvidíte **rozpory mezi meetingy**

**Bonus:** Porovnejte vaši odpověď se sousedem. Liší se? Proč?

**Pointa:** *„Shrň meetingy"* = vágní výstup. *„Extrahuj rozhodnutí do tabulky se sloupci X, Y, Z"* = použitelný výstup.

---

## 🟢 Blok 3 (15 min) — Generování textu

**Cíl:** vyzkoušet, jak AI syntetizuje informace napříč soubory.

**Úkol:**

> *„Napiš markdown dokument: krok-po-kroku návod pro **zaměstnance v kamenných prodejnách**, jak používat QR skener pro vyzvedávání rezervací. Cílová skupina: zaměstnanci 50+ let, **nízká technická zdatnost**. Styl komunikace: vstřícný, trpělivý, žádné anglicismy. Maximálně 1 stránka A4."*

**Po dokončení zkontrolujte:**
- Co se povedlo? A co si agent vymyslel (halucinace)?
- Drží se agent **stylu komunikace** popsaného v `design/design-brief.md`?
- Z jakých souborů agent čerpal informace o tom, **jak rezervace v aplikaci vlastně fungují**? Sedí to s tím, co je v `tech/` a `product/`?

---

## 🟢 Blok 4 (15 min) — Analýza dat

**Cíl:** ověřit, jak agent pracuje s **CSV daty + kontextem**.

**Úkol:** Postupně se zeptejte:

1. > *„Otevři `business/financni-vykonnost.csv`. Jaký je trend tržeb za posledních 5 let? Odpověz konkrétními čísly."*

2. > *„V CSV je sloupec **EBITDA**. Vysvětli mi laicky, co to číslo znamená (jako bys to vysvětloval kamarádovi, který nemá ekonomické vzdělání), a ukaž, jak se EBITDA naší firmy vyvíjelo za posledních 5 let. Co ta čísla říkají o zdraví firmy?"*

3. > *„Vedení firmy schválilo 4,8 mil. Kč na mobilní app. **Dává to ekonomický smysl?** Argumentuj pomocí dat z CSV a kontextu z `business/kpi-prehled.md`. Pokud uvádíš čísla, vždy uveď zdroj."*

**Sledujte:**
- Spustil agent **kód** (Python) nad CSV, nebo si čísla jen odhadl?
- Sedí čísla, která vám dal, s tím, co je opravdu v CSV?
- U úkolu 2: vysvětlí EBITDA **srozumitelně**, nebo jen papouškuje definici z učebnice?
- U úkolu 3: kombinuje agent **CSV s textem** v `kpi-prehled.md`, nebo čte jen jeden zdroj?

**Pointa:** dobrý AI agent kombinuje **strukturovaná data** (CSV) s **kontextem** (markdown). Špatný jen čte jeden zdroj.

---

## 🟢 Blok 5 (10 min) — Kritické myšlení

**Cíl:** najít chyby a rozpory, které lidi obvykle přehlédnou.

**Úkol:** Najděte v repozitáři **alespoň 3 nekonzistence nebo nedořešená rozhodnutí**.

Použijte k tomu agenta, ale **každý nález ověřte čtením souborů sami** (agent může vymyslet rozpor, který tam není).

**Co hledat:**
- Rozpory mezi tím, co padlo v různých poradách
- Otevřené otázky bez vlastníka nebo termínu
- Rozpory mezi popisem aplikace a plánem funkcí
- Funkce, které slibujeme uživatelům, ale není jasné, kdo je dodá
- Rizika, ke kterým chybí řešení

**Příklad promptu, který vám pomůže:**

> *„Projdi všechny soubory v `meetings/`, `product/` a `risks/`. Najdi 5 míst, kde si dokumenty navzájem odporují, nebo kde je rozhodnutí otevřené bez termínu. U každého nálezu uveď přesný citát ze zdrojového souboru a název souboru."*

---

## 🟢 Blok 6 (5 min) — Reflexe (společně)

Diskutujte ve skupině / s lektorem:

1. **Co agent zvládl výborně?**
2. **Kde agent halucinoval?** (Vymyslel si jména, čísla, citáty?)
3. **Která zadání dala nejlepší výstup?** Co měla společné?
4. **Co byste příště zadali jinak?**

**Klíčové ponaučení:**

> *„AI agent je nejvýkonnější, když má **bohatý a strukturovaný kontext**. Soubory jako `CLAUDE.md`, `PRD.md` a meeting notes nejsou jen pro lidi — jsou i pro AI. Investice do dobré dokumentace se vrátí v rychlosti a kvalitě AI výstupů."*

---

## 🔥 Bonusové úkoly (když vám zbude čas)

### B1 — Konkurence
> *„Z `research/konkurence.md` vytáhni 3 největší příležitosti, kde můžeme předehnat Martinus. Argumentuj daty z `business/`."*

### B2 — Commit message
> *„Předstírej, že jsi vývojář, který do projektu přidává funkci **'Seznam přání' (Wishlist)**. Napiš commit message a popis pull requestu pro GitHub ve formátu Conventional Commits."*

### B3 — Risk-based prioritizace
> *„Z `risks/risk-register.md` vyber 3 rizika s nejvyšším Score. Pro každé navrhni **další** mitigaci, která tam ještě není."*

### B4 — Push notifikace
> *„Napiš text push notifikace **'Připravena rezervace v prodejně Praha-Letná'**. Drž se stylu komunikace z `design/design-brief.md`. Vytvoř 3 varianty (krátká, střední, dlouhá)."*

---

## Po hodině — domácí úkol (volitelné)

Vyberte si **jednu funkci** plánovanou pro budoucí verze aplikace (najdete je v sekci 5.3 souboru [product/PRD.md](product/PRD.md) — funkce, které zatím nejsou v první verzi) a s pomocí AI agenta vypracujte:

1. **Popis funkce pro zákazníka** — 1 stránka, jednoduchý jazyk, žádné marketingové fráze
2. **5 otázek a odpovědí (FAQ)**, které zákazník pravděpodobně bude mít
3. **„Co se může pokazit"** — 3 věci, co se mohou zvrtnout, a krátký nápad, jak tomu předejít

Použijte AI agenta na návrh, ale **každý bod osobně zkontrolujte a opravte**. Odevzdejte spolu s krátkou reflexí: **co jste museli opravit a proč**.

---

## Tipy pro celou hodinu

✅ **Zadávejte specificky** — *„Najdi rozhodnutí ze 3 meetingů a udělej tabulku"* je 10x lepší než *„Shrň meetingy"*.

✅ **Ověřujte čísla** — agent může halucinovat. Pokud vám dá konkrétní číslo, najděte ho v souboru.

✅ **Přiložte konkrétní soubory** k promptu (`@cesta/k/souboru`), pokud chcete, aby agent koukal jen tam.

✅ **Když je výstup špatný, neopakujte stejný prompt** — přeformulujte ho. Specifičtěji. S příkladem.

✅ **Sledujte, do jakých souborů agent kouká** — pokud čte málo, dejte mu víc kontextu.

❌ **Nevěřte AI slepě.** Vždy výstup zkontrolujte. Reálný kód, který se nasazuje do produkce, **nikdy** neodevzdejte bez kontroly.
