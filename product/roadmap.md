# Roadmap

**Časový horizont:** Q1 2026 – Q4 2027
**Poslední aktualizace:** 15. března 2026

---

## Q1 2026 — Discovery a design

- ✅ User research (5 hloubkových rozhovorů)
- ✅ Personas a PRD
- ✅ Information architecture
- ✅ Design brief, brand tokens
- ✅ High-fidelity prototyp
- ✅ Tech architektura schválena

## Q2 2026 — Vývoj základů

### Sprint 1 (30.3. – 13.4.)
- Setup repositářů, CI/CD
- Auth modul (backend + iOS + Android)
- Design system v kódu

### Sprint 2 (14.4. – 27.4.)
- Catalog modul (knihy, žánry)
- Vyhledávání (fulltext)
- Detail knihy

### Sprint 3 (28.4. – 11.5.)
- Košík + checkout
- Stripe integrace (Apple Pay, Google Pay, karta)

### Sprint 4 (12.5. – 25.5.)
- Rezervace v prodejně (UI + backend)
- NCR integrace (polling)

### Sprint 5 (26.5. – 8.6.)
- Push notifikace (FCM)
- Notifikační preferences
- Profil a editace údajů

### Sprint 6 (9.6. – 22.6.)
- Věrnostní karta v app
- Apple Wallet + Google Wallet integrace
- Historie objednávek

## Q3 2026 — Stabilizace a launch

### Sprint 7 (23.6. – 6.7.)
- Wishlist
- Sledování objednávek (PPL, Zásilkovna)
- GDPR (export/smazání dat)

### Sprint 8 (7.7. – 20.7.)
- Bug fixing, performance
- Accessibility audit
- Security review (externí)

### Sprint 9 (21.7. – 3.8.)
- Beta testing s 50 zákazníky
- Iterace na základě feedbacku

### Sprint 10 (4.8. – 17.8.)
- Finalizace, App Store / Play Store submission
- Marketingová příprava (Pavel)

### **🚀 Launch: 7. září 2026**

- Soft launch (10% Android rollout, omezený iOS skupina)
- Postupný rollout do 30. září

---

## Q4 2026 — V1.1 (post-launch)

**Tématické zaměření:** kvalita & rozšíření

- Tablet support (Anna používá iPad)
- Audio ukázky (10s ukázka u vybraných titulů)
- Slovenština
- Sdílení knihy externě (Messenger, WhatsApp, e-mail)
- Recenze od komunity (s moderací)
- A/B testovací framework

---

## Q1 2027 — V1.2

**Tématické zaměření:** věrnost & dárky

- Předplatné „Měsíc s knihou" plně v app (zatím jen odkaz na web)
- Dárkové balení + osobní vzkaz při checkoutu
- In-app chat se zákaznickou podporou (Intercom)
- Vouchery a dárkové poukazy
- Notifikace o knihách v oblíbených žánrech od oblíbených autorů

---

## Q2 2027 — V2

**Tématické zaměření:** personalizace & komunita

- Doporučovací engine (ML) — collaborative filtering, content-based
- Knihovní seznamy / booklists
- Kurátorské seznamy editorského týmu
- Profily oblíbených autorů s push notifikacemi
- Citáty z knih a sdílení (jako Goodreads quotes)

---

## Q3–Q4 2027 — Růst

- Možnosti rozšíření v této fázi se rozhodnou na základě dat z V1 a V2:
  - E-knihy?
  - Audioknihy (vlastní, nebo partnerství s Audiotekou)?
  - B2B prostředí (knihovny, firmy)?
  - Polský trh?

---

## Princip prioritizace

Stories se prioritizují podle:

1. **Coverage person:** kolik z 3 person funkce ovlivní pozitivně?
2. **Byznys hodnota:** přinese to konverze, retentioně, NPS?
3. **Technické riziko:** je to dobře pochopené?
4. **Náklady:** kolik to stojí v sprintech?

Vzorec hrubě: `Hodnota = (PersonCoverage × BusinessImpact) / (RiskScore × CostInSprints)`.

## Co se může změnit

Roadmap **není smlouva**. Po každém kvartálu retrospektiva s upravením:

- Pokud V1 nesplní KPI (NPS ≥ 50, konverze ≥ 3%), Q4 se přepíše na kvalitu.
- Pokud konkurence vypustí něco zásadního, reagujeme.
- Pokud uživatelé žádají něco, co není v plánu (např. masivně audioknihy), přepriotizujeme.

## Co se NEzmění

- **Žádné mikroservisy** v horizontu 2 let.
- **Žádný ML před tím, než budeme mít 12 měsíců dat.**
- **Žádné B2B před tím, než stabilizujeme B2C.**
