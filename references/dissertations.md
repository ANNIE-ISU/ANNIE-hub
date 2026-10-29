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
| He | TBD | TBD | *(pending ingestion)* | Foundational detector setup, early Phase I | One of earliest theses; canonical for foundational design decisions and Phase I context |

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
- He thesis: pending upload and ingestion.
