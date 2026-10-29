---
title: "Future Directions"
domain: physics
tags: [future, phase-iii, wurst, wbls, lappd, sbnd, cc1pi, nc, neutron-multiplicity, snapshot-2024]
links_to:
  - detector/lappd.md
  - physics/neutrino-interactions.md
  - detector/water-cherenkov-detector.md
last_updated: "2026-10-29"
sources:
  - "He, Julie. LAPPD Integration in ANNIE. UC Davis PhD Dissertation, 2024. Ch. 7."
---

# Future Directions


> ⚠️ **Snapshot article** — this content reflects the outlook as of **He 2024** and is expected to be superseded by subsequent dissertations (Yue and others). When ingesting newer work, update or replace sections here rather than treating these as current plans. Apply the temporal authority rule: newer dissertations win on current intentions.

This article captures the near- and long-term physics and hardware goals for the ANNIE experiment, as described in He 2024 Chapter 7. It is a living document — update as plans evolve.

---

## Near-Term Analysis Goals

### Improving the Neutron Multiplicity Measurement

The CC-0π neutron multiplicity result in He 2024 has several known sources of systematic uncertainty to address:

- **Neutron detection efficiency** — the AmBe efficiency map (55–70% position-dependent) contributes the dominant systematic uncertainty; a new campaign with the BGO + PMT configuration (replacing the SiPM pair) is in progress to produce an updated map
- **Background characterization** — external neutrons, cosmic spallation, and radioactive isotopes remain to be quantified (see [backgrounds](backgrounds.md))
- **LAPPD data stream** — tools to integrate LAPPD timing information into the vertex reconstruction and multiplicity analysis are under active development at the time of He 2024

### Extending to Other Event Topologies

He 2024 covers only **CC-0π** events (charged-current, no pions). The following topologies are also accessible with the ANNIE dataset:

| Topology | Description | Sensitivity |
|---|---|---|
| CC-0π | νμ + O → μ⁻ + X, no pions | ✅ Published (He 2024) |
| CC-1π | νμ + O → μ⁻ + π + X | Planned |
| NC | Neutral current events | Planned |

### Joint Analysis with SBND

ANNIE is positioned to perform a **joint cross-section measurement on water** with the Short-Baseline Near Detector (SBND) at Fermilab, exploiting the shared BNB beam and complementary detector technologies.

---

## LAPPD System Expansion

The nominal ANNIE Phase II configuration is **5 LAPPDs**. He 2024 notes that ANNIE may install additional LAPPDs beyond this nominal number if the physics case and resources support it. Each additional LAPPD:
- Improves vertex resolution (38 cm → 12 cm with 5 LAPPDs; further improvement expected with more)
- Extends the fiducial volume for fast-timing reconstruction
- Provides redundancy against individual unit failures

The LVHV board will require further revisions to support a larger LAPPD array — specifically the UV/OV protection window and the high-voltage zener clamp voltages identified as issues in v2 (see [LAPPD engineering history](../detector/lappd.md#engineering-history)).

---

## ANNIE Phase III: ANNIE WURST

### Background

ANNIE has already demonstrated that **Cherenkov light and scintillation light can be separated by timing** — a foundational capability for next-generation water-based detectors. This demonstration used the **SANDI vessel** filled with **5% water-based liquid scintillator (WbLS)** installed inside the ANNIE tank.

### The WURST Concept

**ANNIE WURST** = *ANNIE WbLS Underwater Retrievable Scintillator Tank*

Phase III would deploy a **balloon-like structure** filled with WbLS — possibly gadolinium-doped — inside the existing ANNIE water tank. The design is modeled on the **KamLAND** detector concept, where a spherical balloon of liquid scintillator is suspended inside a larger water-filled vessel.

| Aspect | Description |
|---|---|
| Fill medium | Water-based liquid scintillator (WbLS), possibly Gd-doped |
| Structure | Retrievable balloon inside the ANNIE tank |
| Separation | Cherenkov vs. scintillation using fast timing (LAPPDs critical) |
| Physics motivation | Improved event reconstruction, supernova sensitivity, solar neutrino potential |
| Precedent | KamLAND; SANDI WbLS demonstration in ANNIE |

### Why WbLS?

WbLS offers Cherenkov light (for directionality) and scintillation light (for total energy and low-threshold detection) in a single medium. With ANNIE's LAPPDs providing <100 ps timing resolution, the two light components can be separated event-by-event, enabling reconstruction capabilities unavailable in pure water or pure scintillator detectors.

---

## See Also

- [Neutrino Interactions](neutrino-interactions.md) — current measurement results
- [LAPPD](../detector/lappd.md) — LAPPD hardware and commissioning
- [Water Cherenkov Detector](../detector/water-cherenkov-detector.md) — ANNIE tank and water system
- [Calibration](../detector/calibration.md) — AmBe neutron efficiency map
