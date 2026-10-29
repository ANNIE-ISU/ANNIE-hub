# Dissertations and Theses

**Domain:** References  
**Links to:** [INDEX](../INDEX.md)  
**Last updated:** 2026-10-28

---

## Purpose

This file indexes all PhD dissertations and Master's theses that contribute to the ANNIE-hub knowledge base. Entries include the citation, key topics covered, and the authority weight assigned when sourcing wiki articles.

**Temporal authority rule:** When two sources conflict on a factual or technical detail, the *newer* source takes precedence. Older sources remain canonical for historical context (e.g., describing the state of the experiment at a given phase) and are always cited when discussing that period.

---

## Indexed Dissertations

| Author | Year | Institution | Title (abbreviated) | Key Topics | Authority Notes |
|--------|------|-------------|---------------------|------------|-----------------|
| Nieslony | 2022 | Univ. Mainz | *Towards a neutron multiplicity measurement with ANNIE* | Detector subsystems (Ch. 3), MRD/FMV efficiencies (Ch. 4), Simulation validation (Ch. 5), Neutron multiplicity (Ch. 6), DSNB study (Ch. 7), WbLS energy reconstruction (Ch. 8) | Most recent Phase II detector description; primary source for neutron multiplicity, efficiency measurements, simulation validation methodology |
| He | 2024 | UC Davis | *Neutron Multiplicity Measurement in the ANNIE Detector* | Detector overview Ch.3 (PMT inventory, FMV/MRD, BNB beam, Gd loading, triggers, ToolAnalysis); LAPPD hardware Ch.4; Neutron multiplicity analysis Ch.6 (ring-edge vertex, RNN, Bethe-Bloch energy, neutron cuts, AmBe efficiency, Table 6.1) | **Supersedes Nieslony 2022** on all overlapping factual/technical details (temporal authority: newer wins); advisor: Svoboda |

---

## Citation Format

When citing a dissertation in a wiki article, add the following YAML to the article's frontmatter:

```yaml
---
sources:
  - author: "Nieslony"
    year: 2022
    type: dissertation
    institution: "Univ. Mainz"
    notes: "Chapters 3–6 primary source"
---
```

---

## Ingestion Notes

- Nieslony 2022 was ingested in full (10 chapter PDFs) in October 2026. All chapter-level extractions are logged in the wiki articles they sourced.
- He 2024 (Julie He, UC Davis, advisor Svoboda): Chapters 3, 4, and 6 ingested October 2026. Ch.3 → detector subsystem articles; Ch.6 → neutrino-interactions.md with methods and Table 6.1 results. Ch.4 (LAPPD hardware) pending full wiki article.
