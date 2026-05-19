# Risk Register

**Vlastník:** Tereza Marešová (PM)
**Aktualizováno:** 18. března 2026
**Frekvence revize:** každý sprint review

---

## Klasifikace

- **Impact:** 1 (zanedbatelný) – 5 (kritický)
- **Probability:** 1 (velmi nepravděpodobné) – 5 (téměř jisté)
- **Score:** Impact × Probability (1–25)
- **Status:** Open / Mitigating / Closed / Realized

---

## Aktivní rizika

### R-01 — NCR pokladny nemají stabilní API
- **Kategorie:** Technické / Integrace
- **Impact:** 5 (bez tohoto MVP nefunguje)
- **Probability:** 3
- **Score:** 15
- **Status:** Mitigating
- **Popis:** NCR pokladny z 2017 nemají oficiální dokumentaci, jen interní wiki Ládi z IT. Neznámé rate limity, neznámá stabilita.
- **Mitigace:**
  - Sprint 2: pilot polling jen na 1 prodejně (Praha-Letná)
  - Monitorování chyb, latence, retry logiky
  - Fallback: pokud polling padá, ukázat „Dostupnost ve [Mesto] — kontaktujte prodejnu" místo selhání
- **Vlastník:** Martin Doležal
- **Trigger:** Sprint 2 retro 27. 4.

### R-02 — Deadline Q3 2026 nereálný
- **Kategorie:** Plánování
- **Impact:** 4 (zklamání stakeholderů, tlak na kvalitu)
- **Probability:** 4 (po Sprintu 1 odhady nesedí)
- **Score:** 16
- **Status:** Mitigating
- **Popis:** Sprint 1 ukázal, že velocity je nižší než plánovaný odhad. Pokud trend pokračuje, MVP nebude do Q3 2026 hotový v plném scope.
- **Mitigace:**
  - Tereza pravidelně komunikuje status Jiřímu (bi-weekly)
  - Připraven scope-cut plán: pokud Sprint 5 ukáže slip, vyhodí se Wishlist (P1) a Sledování objednávek (P1)
  - Aktivně chráníme P0 stories
- **Vlastník:** Tereza
- **Trigger:** každá retrospektiva

### R-03 — Apple App Store review zamítne app
- **Kategorie:** External
- **Impact:** 5 (delay launchu)
- **Probability:** 2
- **Score:** 10
- **Status:** Open
- **Popis:** Apple může zamítnout app kvůli: chybějícímu Sign In with Apple (povinné, když máme Google), nedostatečným privacy disclosures, performance issues.
- **Mitigace:**
  - Sign in with Apple je v MVP (P0)
  - Privacy nutrition labels připraveny ve Sprintu 8
  - Internal review podle Apple Review Guidelines před submission
  - První submission 6 týdnů před launchem (rezerva)
- **Vlastník:** Jana Pokorná

### R-04 — Magic linky končí v spamu
- **Kategorie:** Technické / Doručitelnost
- **Impact:** 4 (špatné UX, dropped funnels)
- **Probability:** 3
- **Score:** 12
- **Status:** Mitigating
- **Popis:** Magic link je jediný způsob registrace e-mailem. Pokud končí v spamu, uživatel se nepřihlásí.
- **Mitigace:**
  - Postmark (vyšší doručitelnost než Mailgun)
  - SPF, DKIM, DMARC nakonfigurované
  - Domain warming před launchem (postupné posílání 4 týdny předem)
  - Subject line A/B test (3 varianty)
  - Monitoring open rate; pokud < 70 %, vyšší alert
- **Vlastník:** Martin

### R-05 — Zaměstnanci v prodejnách neumí používat skener QR
- **Kategorie:** Operations
- **Impact:** 3
- **Probability:** 4
- **Score:** 12
- **Status:** Mitigating
- **Popis:** Průměrný věk zaměstnanců v prodejnách 47 let, část z nich má potíže s technologií. Pokud nesehnem QR skenování, rezervace selhávají.
- **Mitigace:**
  - Lucie + Tereza: fyzický 1-stránkový návod u každé pokladny
  - Video tutorial 3 min, povinný před launchem
  - „Champion" v každé prodejně (1 osoba, která rozumí, pomáhá ostatním)
  - Hotline na IT support první měsíc (Láďa + Tereza)
- **Vlastník:** Lucie Horáková

### R-06 — Marže klesá rychleji než app zachytí
- **Kategorie:** Byznys
- **Impact:** 4
- **Probability:** 3
- **Score:** 12
- **Status:** Open
- **Popis:** Hrubá marže klesla z 34,2 % (2021) na 30,4 % (2025). Pokud trend pokračuje, app launch v 09/2026 přijde do firmy, která je ekonomicky napjatá.
- **Mitigace:**
  - App primárně zachytí mobilní konverzi (≥ 28 mil. Kč/rok dle [kpi-prehled.md](../business/kpi-prehled.md))
  - Věrnostní program zvedne retentioně → opakovaný nákup
  - Není v naší kompetenci řešit marže jako celek
- **Vlastník:** Hana Agentovská (CFO)

### R-07 — Konkurence (Martinus) vypustí podobnou feature dřív
- **Kategorie:** Tržní
- **Impact:** 3
- **Probability:** 2
- **Score:** 6
- **Status:** Open
- **Popis:** Martinus může rozšířit kamenné prodejny a vypustit „rezervace do 2h" feature, čímž ubere naši unikátní výhodu.
- **Mitigace:**
  - Naše výhoda je 14 prodejen v krajských městech — Martinus má jen 1 v Praze
  - Reálně bude Martinusu trvat 2+ roky, než tuto síť vybuduje
  - Sledování news o Martinusu (Pavel quarterly report)
- **Vlastník:** Pavel Novotný

### R-08 — Stripe nedostane v ČR Apple Pay v termínu
- **Kategorie:** External
- **Impact:** 3
- **Probability:** 1
- **Score:** 3
- **Status:** Open
- **Popis:** Apple Pay přes Stripe v ČR funguje, ale teoreticky může mít regionální problémy.
- **Mitigace:**
  - Apple Pay už ve Sprintu 3 (test)
  - Fallback: jen platba kartou (degraded UX, ale funguje)
- **Vlastník:** Martin

### R-09 — Údaje uživatelů uniknou (security incident)
- **Kategorie:** Bezpečnost / Legal
- **Impact:** 5 (GDPR pokuta, reputace)
- **Probability:** 1
- **Score:** 5
- **Status:** Mitigating
- **Popis:** Únik dat by způsobil GDPR pokutu (až 4 % obratu = až 8 mil. Kč) a poškodil reputaci značky.
- **Mitigace:**
  - Žádná hesla v DB (magic link)
  - Encryption at rest (RDS), in transit (HTTPS)
  - Pravidelný npm audit + Snyk v CI
  - Externí security audit ve Sprintu 8 (rozpočet 80k Kč)
  - Incident response plán (TODO Sprint 7)
- **Vlastník:** Martin

### R-10 — Předplatné „Měsíc s knihou" v MVP jen jako odkaz na web
- **Kategorie:** UX / Byznys
- **Impact:** 2
- **Probability:** 5 (jisté, takhle to je)
- **Score:** 10
- **Status:** Realized (accepted)
- **Popis:** Z kickoffu vyplynulo, že předplatné nebude v MVP. Riziko: uživatelé, kteří otevřou předplatné v app, dostanou jen odkaz na web — špatný UX.
- **Mitigace:**
  - Pečlivý copy: „Předplatné spravujete na našem webu, otevře se v prohlížeči."
  - Deep link s pre-fillem účtu (single sign-on na webu)
  - V1.2 plně v app
- **Vlastník:** Tereza

### R-11 — Doporučovací engine — Jiří chce, tým nemá kapacity
- **Kategorie:** Stakeholder management
- **Impact:** 2
- **Probability:** 4
- **Score:** 8
- **Status:** Open
- **Popis:** Z [kickoffu](../meetings/2025-11-12-kickoff.md): Jiří nakonec řekl „aspoň jednoduché doporučení tam musí být". Tereza měla jednat, ale stav je stále otevřený.
- **Mitigace:**
  - Tereza naplánovaná 1:1 s Jiřím 27. 3.
  - Návrh kompromisu: „Bestsellery v žánru" (čisté SQL, ne ML) = 1 sprint
  - Pokud Jiří trvá na pokročilejším, eskalovat na Hanu (CFO) kvůli rozpočtu
- **Vlastník:** Tereza

### R-12 — Tablet support — odložený, ale uživatelé budou žádat
- **Kategorie:** Scope
- **Impact:** 2
- **Probability:** 3
- **Score:** 6
- **Status:** Open
- **Popis:** iPad / Android tablety jsou na MVP odložené (V1.1). Uživatelka Anna z persona explicitně používá iPad.
- **Mitigace:**
  - Layout responsive (SwiftUI / Compose to ulehčí)
  - V1.1 Q4 2026 plně tablet
  - V mezičase: app na iPadu funguje v iPhone-mode (ne ideal, ale funguje)
- **Vlastník:** Kateřina + Jana

---

## Uzavřená rizika (closed)

### R-00 — Tech stack: nativní vs. cross-platform
- **Status:** Closed (vyřešeno v kickoffu)
- **Decision:** Nativní iOS + Android
- **Důvody:** lepší Apple Pay/Wallet integrace, výkon, dlouhodobá udržitelnost

---

## Reportování

- Top 3 rizika podle Score se diskutují na **každém PM standupu** (út, čt)
- Plný register se reviewuje na konci každého sprintu (bi-weekly)
- Eskalace na Jiřího (CEO) když: Score ≥ 16 nebo Status změna na Realized

## Reference

- [PRD.md](../product/PRD.md)
- [Tech architecture](../tech/architecture.md)
- [Kickoff meeting](../meetings/2025-11-12-kickoff.md)
- [Tech planning](../meetings/2026-03-18-tech-planning.md)
