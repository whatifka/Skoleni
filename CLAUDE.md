# Instrukce pro AI agenty

Tento repozitář obsahuje **dokumentaci a plánovací materiály** pro novou mobilní aplikaci knihkupectví Knihy Agentovský. Zatím **neobsahuje žádný produkční kód** — jsme ve fázi discovery a plánování.

## Kde co hledat

| Když potřebuješ... | Podívej se do... |
|---|---|
| Vědět, co projekt dělá | [README.md](./README.md) |
| Pochopit byznys kontext | [business/](./business/) |
| Pochopit rozhodnutí týmu | [meetings/](./meetings/) |
| Vědět, co se má postavit | [product/PRD.md](./product/PRD.md) |
| Pochopit cílového uživatele | [product/personas.md](./product/personas.md) |
| Vědět, jak má vypadat UI | [design/design-brief.md](./design/design-brief.md) |
| Pochopit technické rozhodnutí | [tech/](./tech/) |
| Vědět, čeho se bát | [risks/risk-register.md](./risks/risk-register.md) |

## Jazyk

Veškerá dokumentace je **v češtině**. Pokud generuješ nové artefakty (PRD, user stories, commit messages, kód s komentáři), drž se češtiny, pokud uživatel neřekne jinak. Identifikátory v kódu (názvy proměnných, tříd, funkcí) piš anglicky podle běžné konvence.

## Co tým očekává

- **Před implementací:** přečti si relevantní meeting transcripts a PRD. Mnoho rozhodnutí už padlo — neptej se znovu na to, co je zdokumentované.
- **Nekonzistence:** v meeting transcripts jsou záměrně rozpory mezi rozhodnutími (např. mezi kickoff meetingem a pozdějšími poradami). Pokud na rozpor narazíš, **upozorni na něj** místo toho, abys volil jednu variantu naslepo.
- **Cílovka:** B2C, věk 25–55, čtenáři. Drž tone of voice **přátelský, ne příliš formální, knihomilský**.

## Co tým netoleruje

- Návrhy řešení, která ignorují rozpočet uvedený v [PRD](./product/PRD.md)
- Technologická rozhodnutí, která jdou proti tomu, co je v [tech/architecture.md](./tech/architecture.md), bez explicitního zdůvodnění
- Generování fiktivních dat o reálných knihách bez označení, že jsou smyšlená
