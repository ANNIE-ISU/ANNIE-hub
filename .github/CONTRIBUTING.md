# Contributing to ANNIE-Hub

This wiki works best when everyone contributes. Here are the conventions to follow so articles stay consistent and AI-readable.

---

## Core Principles

1. **Atomic entries** — each file covers exactly one concept, system, or decision. If it needs to cover two things, split it.
2. **Interlink everything** — every article should link to related articles. An isolated page is a dead end.
3. **Update the index** — when you add a new article, add it to [INDEX.md](../INDEX.md).
4. **Date your updates** — keep the `Last updated:` field current in every article you touch.
5. **Prefer prose over bullet soup** — write in complete sentences where it matters. Bullets are fine for lists of items, not for explanations.

---

## Article Header Format

Every article should start with this block:

```markdown
# Article Title

**Domain:** [Detector / Physics / Software / Projects / People / Meetings]  
**Links to:** [Article](path.md) · [Article](path.md)  
**Last updated:** YYYY-MM-DD

---
```

---

## File Naming

- Use lowercase, hyphen-separated names: `photon-detection-system.md`
- Decision log entries: `YYYY-MM-DD-short-description.md`
- Meeting notes: `YYYY-MM-DD.md` (weekly) or `YYYY-MM.md` (monthly)

---

## What Belongs Here vs. Elsewhere

| Content type | Where it goes |
|---|---|
| Conceptual explanation of a system | This wiki |
| "Why we decided X" | `projects/decisions/` |
| Code | The relevant code repo |
| Raw data | Fermilab storage / computing cluster |
| Draft paper / analysis note | *[wherever the group keeps those]* |

---

## AI Agent Notes

This wiki is designed to be read by AI agents (Claude, Copilot, etc.) as well as humans. When writing:
- Be explicit rather than implicit — don't assume the reader knows the local jargon
- Define acronyms on first use in each article
- Keep `INDEX.md` accurate — it's the agent's entry point

---

*Questions? Ask Matt Wetstein or open a GitHub Issue.*
