# Software Overview

**Domain:** Software  
**Links to:** [DAQ System](daq-system.md) · [Simulation Framework](simulation-framework.md)  
**Last updated:** 2026-10-26

---

## Software Ecosystem Map

ANNIE's software stack spans data acquisition, simulation, reconstruction, and analysis. This article maps how the pieces fit together.

```
BNB beam spill
      │
      ▼
[Trigger System] ──→ [DAQ / ToolDAQ] ──→ raw data files
                                               │
                              ┌────────────────┤
                              ▼                ▼
                     [Simulation / WCSim]   [Reconstruction]
                              │                │
                              └────────┬───────┘
                                       ▼
                                  [Analysis]
```

## Key Frameworks

| Framework | Purpose | Repo / Location |
|---|---|---|
| ToolDAQ / ToolAnalysis | DAQ and analysis framework | *[add link]* |
| WCSim | Water Cherenkov simulation | *[add link]* |
| GENIE | Neutrino interaction generator | *[add link]* |
| RATPAC / *[other]* | *[add if applicable]* | *[add link]* |

## Computing Resources

*[Fill in: where jobs are run — Fermilab grid, local cluster, XSEDE, etc.]*

## Getting Started

*[Fill in: onboarding steps for a new student to get the software environment set up.]*

---

*See also: [DAQ System](daq-system.md) · [Simulation Framework](simulation-framework.md)*
