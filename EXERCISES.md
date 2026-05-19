# Cvičení pro studenty (1 hodina) — manuál lektora

Tento soubor je **pro lektora**, ne pro studenty. Obsahuje detailní plán 60minutového cvičení s tipy, co očekávat, a jak studenty navést.

> Studenty otevřou tento repozitář v Claude Code (nebo jiném AI agentu) a budou ho používat jako zdroj kontextu. Repozitář **nemá produkční kód** — je to čistě dokumentační kontext.

---

## Cíle cvičení

Po hodině studenti mají:

1. **Pochopit**, jak AI agent skládá kontext z více souborů.
2. **Umět** AI agentovi zadávat úkoly, které vyžadují syntézu napříč dokumenty.
3. **Vědět, kde AI selhává** (halucinace, nekonzistence) a jak ho korigovat.
4. **Mít praktickou zkušenost** s generováním produktových artefaktů (PRD, user stories, analýzy).

## Co potřebujete

- Claude Code (nebo jiný AI agent — Cursor, Windsurf, Aider) nainstalovaný u každého studenta
- Tento repozitář naklonovaný lokálně
- Projektor pro společnou diskuzi

---

## Časový plán (60 min)

### 🟢 0–5 min — Orientace (jednotlivě)

**Úkol pro studenty:**
> *„Otevřete repozitář v Claude Code. Zeptejte se: 'O čem je tento projekt? Shrň mi to.' Sledujte, do jakých souborů agent kouká."*

**Co učitel komentuje:**
- Agent typicky čte `README.md` → `CLAUDE.md` → `business/` → `product/PRD.md`
- Zdůrazněte: **CLAUDE.md je první-class citizen** pro agenty
- Diskuze: čím lepší je dokumentace, tím lepší odpověď

**Časté problémy:**
- Studenti zapomenou, že musí být v adresáři repozitáře — pomozte jim
- Někteří agenti přečtou jen 1–2 soubory — to je normální, to je důvod, proč píšeme `CLAUDE.md`

---

### 🟢 5–15 min — Extrakce poznatků (jednotlivě, 10 min)

**Úkol pro studenty:**
> *„Najdi a shrň všechna rozhodnutí, která padla ve třech meetinzích. Vytvoř tabulku: rozhodnutí | meeting | kdo navrhl | status."*

**Co studenti zjistí:**
- Některá rozhodnutí jsou rozporuplná (např. deadline Q2 vs. Q3, doporučovací engine ano/ne)
- Některé akční body z kickoffu nebyly nikdy dořešené
- Agent typicky najde 10–15 rozhodnutí

**Diskuze (5 min):**
- Porovnejte 3 výstupy studentů — kdo dostal lepší tabulku?
- Co se stane, když řekneme „shrň meetingy" vs. „extrahuj rozhodnutí"? **Specifické zadání = specifická odpověď.**
- Najděte alespoň 1 rozpor mezi meetingy

---

### 🟢 15–30 min — Generování artefaktu (dvojice, 15 min)

Rozdělte studenty na dvojice. Každá dvojice dostane **jeden ze 3 úkolů** (nebo si volí):

**Varianta A:**
> *„Z meeting transcripts a design briefu vygeneruj draft PRD pro funkci 'Audio ukázky knih', která má jít do V1.1. Použij stejnou strukturu, jakou má hlavní PRD.md."*

**Varianta B:**
> *„Vytvoř 10 user stories pro epic 'Wishlist' (US-060 a další). Použij notaci Given/When/Then. Drž se priorit a velikostí z user-stories.md."*

**Varianta C:**
> *„Napsal/a jsi novou stránku pro 'Onboarding employee v prodejně' (jak používat QR skener). Vytvoř markdown dokument s krok-po-kroku tutoriálem, který Lucie pošle zaměstnancům. Tone of voice musí odpovídat zaměstnancům 50+."*

**Diskuze (závěr 15 min):**
- Kde agent halucinoval? (Často vymyslí features, které nejsou v PRD)
- Kdo dostal nejlepší tone of voice? Proč?
- **Klíčový takeaway:** AI je dobré v draftu, ne ve finální verzi. Vždy reviewovat.

---

### 🟢 30–45 min — Analýza dat (jednotlivě, 15 min)

**Úkol pro studenty:**
> *„Otevři `business/financni-vykonnost.csv`. Zeptej se Claude:
> 1. 'Jaký je trend tržeb, marže a EBITDA za 5 let?'
> 2. 'Vypočti CAGR pro tržby.'
> 3. 'Dává smysl investovat 4,8 mil. Kč do mobilní app, když EBITDA klesá? Argumentuj pomocí dat.'"*

**Co studenti zjistí:**
- Agent umí číst CSV a počítat statistiky (i bez spouštění kódu)
- Některé agenty pustí Python a opravdu spočítají; jiné jen odhadnou — porovnejte
- **3. otázka je trick:** kvalitní odpověď musí kombinovat CSV s [kpi-prehled.md](./business/kpi-prehled.md), kde je vysvětlení trendu

**Diskuze:**
- Kdo dostal odpověď s konkrétními čísly (CAGR % vypočtené správně)?
- Kdo dostal jen „obecnou analýzu" bez čísel?
- **Lekce:** zadávat konkrétně, žádat čísla, ne dojmy

---

### 🟢 45–55 min — Kritické myšlení (skupiny po 3–4, 10 min)

**Úkol pro skupiny:**
> *„Najděte v repozitáři **3 nekonzistence nebo nedořešená rozhodnutí**. Použijte k tomu Claude, ale výstup ověřte čtením souborů sami.
> Příklady, co hledat: rozpory mezi meetingy, otevřené otázky, akční body bez vlastníka, rozpory mezi PRD a roadmapou."*

**Co tam záměrně je (spoiler pro lektora):**

1. **Deadline:** Jiří v kickoffu trval na Q2 2026. PRD říká Q3 2026. Roadmap potvrzuje Q3 (launch 7. září 2026). Tech planning meeting říká „posunuto po Sprint 1 retru".
2. **Doporučovací engine:** vyloučen v kickoffu, ale Jiří „aspoň jednoduché". V PRD je jako P1 (F-12). V risks (R-11) stále otevřené.
3. **Wishlist:** v kickoffu vyloučený, ale Tereza protestovala („wishlist je základ!"). V PRD je teď jako F-11 (přidaný v Sprint 2 retro). User stories ho mají jako Epic E7.
4. **Předplatné „Měsíc s knihou":** v MVP jen jako odkaz na web (kickoff). Není dořešené, jak to bude UX vypadat. Risk R-10 to akceptuje.
5. **Audio ukázky:** vyloučené v kickoffu, ale v design review byla otevřená otázka OQ-3 „bez nich UX vypadá chudě". V PRD nejsou, v roadmapě jsou v Q4 2026 (V1.1).
6. **Tablet support:** Anna persona explicitně používá iPad, ale tablet support odložený na V1.1. Risk R-12.
7. **Marketing launch:** Pavel se ptal v kickoffu, Tereza řekla „máme půl roku". Není v risks, není v roadmapě.
8. **Otevřená otázka OQ-2 v PRD:** „Co s expirací věrnostních bodů?" — stále otevřená.

**Diskuze (3 min):**
- Skupiny prezentují, co našly
- Lektore: pochval, pokud najdou méně očekávané rozpory
- **Klíčový takeaway:** AI agent **najde víc nekonzistencí než člověk při rychlém čtení** — je to skvělý reviewer dokumentace

---

### 🟢 55–60 min — Reflexe (5 min, společně)

Otázky pro diskuzi:

1. **Co agent zvládl dobře?**
2. **Kde halucinoval?** (Typicky: vymyslí jména prodejen, ceny knih, percentage; vymyslí features nebo lidi co neexistují)
3. **Kdy byla potřeba korekce?**
4. **Co byste příště zadali jinak?**

**Závěrečný takeaway:**

> *„AI agent je nejvýkonnější, když má **bohatý a strukturovaný kontext**. Dokumentace v repozitáři není pro lidi — je i pro AI. Investice do `CLAUDE.md`, `PRD.md`, do meeting notes se vrátí v rychlosti a kvalitě AI výstupů."*

---

## Bonusová cvičení (pro rychlejší studenty nebo pokud zbude čas)

### B1 — Konkurenční analýza
> *„Z `research/konkurence.md` vytáhni 3 největší příležitosti, kde můžeme předehnat Martinus. Argumentuj daty z business/."*

### B2 — Git workflow
> *„Předstírej, že jsi PR autor. Napiš commit message a PR description pro přidání funkce 'Wishlist' (US-060). Drž se Conventional Commits formátu."*

### B3 — Risk-based prioritization
> *„Z `risks/risk-register.md` vyber 3 rizika s nejvyšším Score. Pro každé navrhni dodatečnou mitigaci."*

### B4 — Code scaffold
> *„Na základě `tech/api-spec.md` vygeneruj Express + TypeScript boilerplate pro endpoint `POST /reservations`. Použij Zod pro validaci."*

### B5 — Marketing copy
> *„Napiš text push notifikace pro 'Připravena rezervace v prodejně Praha-Letná'. Drž se tone of voice z design briefu. Vytvoř 3 varianty."*

---

## Tipy pro lektora

### Před hodinou
- Otestujte si všechna zadání s vlastním AI agentem — odhalte, kde to drhne
- Mějte připravený seznam **8 nekonzistencí** (viz výše) pro diskuzi
- Pokud máte projektor, sdílejte svou Claude Code session během vysvětlování

### Během hodiny
- **Neradějte hned řešení.** Nechte studenty narazit na limity AI sami — z toho se nejvíc naučí.
- Pokud někdo dostane brilantní výstup, **požádejte ho, ať sdílí prompt** s ostatními.
- Sledujte, kdo používá víc kontextu (více souborů přiložených, lepší zadání) — zdůrazněte rozdíl.

### Časté otázky studentů

**„Proč si agent vymyslel jméno autora knihy?"**
> Halucinace. AI doplňuje pravděpodobné odpovědi, když nemá data. V repozitáři jsou jen kategorie a struktury, ne konkrétní knihy. Učte studenty, aby agenta opravili: „Tato kniha v repu není, nepoužívej fiktivní data."

**„Proč mi agent odmítl odpovědět?"**
> Pravděpodobně narazil na bezpečnostní pravidlo, nebo nemá kontext. Ukázat, jak doplnit kontext (manuálně přiložit soubor, nebo explicitně odkázat).

**„Mám použít jiný model?"**
> Diskuze: Sonnet vs. Haiku vs. Opus. Pro tato cvičení stačí Sonnet 4.6. Opus je overkill, Haiku může dělat víc chyb v syntéze.

---

## Po hodině — domácí úkol (volitelný)

> *„Vyberte si jednu funkci ze sekce 5.3 PRD (NENÍ v MVP) a vypracujte pro ni:
> 1. Návrh PRD sekce (jak by se rozšířil hlavní PRD)
> 2. 5 user stories v notaci Given/When/Then
> 3. Risk analysis (3 rizika)
> Vše vygenerujte s pomocí AI agenta, ale **každou sekci osobně zreviewujte a opravte**. Odevzdejte spolu s krátkou reflexí: co jste museli opravit a proč."*

---

## Materiály k dalšímu studiu

- [Anthropic — Prompt engineering best practices](https://docs.anthropic.com/claude/docs/prompt-engineering)
- [The 70% problem with AI-generated code](https://addyosmani.com/blog/ai-generated-code/) — kde AI selhává
- [Karpathy — context is king](https://twitter.com/karpathy) — proč je kontext důležitější než model
