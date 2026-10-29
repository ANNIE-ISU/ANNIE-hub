# Detector Overview

**Domain:** Detector  
**Links to:** [Water Cherenkov System](water-cherenkov-detector.md) · [Photon Detection System](photon-detection-system.md) · [Trigger System](trigger-system.md) · [Physics Overview](../physics/overview.md)  
**Last updated:** 2026-10-28

---
sources:
  - author: "Nieslony"
    year: 2022
    type: dissertation
    institution: "Univ. Mainz"
    notes: "Chapter 3 primary source for detector geometry and subsystem specs; Chapter 4 for subsystem efficiencies"
---

## What ANNIE Is

ANNIE (Accelerator Neutrino Neutron Interaction Experiment) is a 26-ton gadolinium-loaded water Cherenkov neutrino detector located at Fermilab on the Booster Neutrino Beamline (BNB), in the SciBooNE hall (Building 109), approximately 100 m downstream from the BNB target. Its scientific goals are to measure neutron production and CC/NC cross sections in neutrino-nucleus interactions, and to serve as a testbed for advanced detection technologies — particularly Large-Area Picosecond PhotoDetectors (LAPPDs) and novel target media.

---

## Detector Geometry (Phase II)

### Water Tank

The ANNIE water tank is a steel cylinder with approximate dimensions:

- **Radius:** ~1.5 m
- **Height:** ~4 m
- **Target mass:** 26 tons (active volume, Gd-loaded water)
- **Gd loading:** 0.2% Gd₂(SO₄)₃ by mass

The tank contains an octagonal **Inner Structure** (steel frame, Teflon-wrapped for reflectivity) that mounts the PMTs and LAPPD insertion columns. A black plastic liner optically isolates the Inner Structure from the outer tank wall.

### Photon Detectors

132 PMTs total — see [Photon Detection System](photon-detection-system.md) for full breakdown by type and position. 5 LAPPDs planned; first deployed March 2022.

### Muon Range Detector (MRD)

Located downstream of the water tank. A scintillator-steel sandwich structure used to range out muons and reconstruct their energy and direction.

| Parameter | Value |
|-----------|-------|
| Scintillator paddles | 306 total |
| Horizontal layers | 5 |
| Vertical layers | 6 |
| Paddle types | SciBooNE (20 cm wide) + KTeV (15 cm wide) |
| Steel absorber layers | 11 layers, ~5.1 cm thick each |
| Readout | 12 CAMAC TDC units |
| Acquisition window | 4 μs |
| Origin | Recycled from SciBooNE and KTeV experiments |

### Front Muon Veto (FMV)

Located upstream of the water tank. Identifies muons originating outside the detector ("dirt muons") to separate them from neutrino interaction candidates.

| Parameter | Value |
|-----------|-------|
| Layers | 2 |
| Paddles per layer | 13 |
| Total paddles | 26 |
| Paddle height | 31.1 cm |
| Total width | 322 cm |
| Logic | OR between two layers |
| Origin | Recycled from CDF experiment |

---

## Subsystem Efficiencies (Phase II, 2021 data)

Measured in-situ using through-going muon tracks (Nieslony 2022, Chapter 4).

| Subsystem | Efficiency | Notes |
|-----------|------------|-------|
| FMV (tagging a dirt muon) | **95.6 ± 1.6%** | Average miss probability 4.4 ± 1.6%; OR of 2 layers; consistent across Jan–Jun 2021 running |
| MRD (all instrumented paddles) | **90.3 +9.7/−14.9%** | Layers 2–10; Layers 1 and 11 excluded (coincidence requirement) |
| MRD (active channels only) | **92.1 ± 7.9%** | Excluding 3.9% of paddles turned off due to PMT issues |

---

## BNB Beam Parameters

| Parameter | Value |
|-----------|-------|
| Proton kinetic energy | 8 GeV (momentum 8.89 GeV/c) |
| Target | Beryllium (at MI-12) |
| Max intensity | 5 × 10¹² protons/pulse |
| Repetition rate | up to 5 Hz |
| Bunch structure | 81 of 84 Booster bunches filled; bunches 2 ns wide, 19 ns apart |
| Spill duration | ~1.6 μs |
| Beam composition (ν-mode) | 93.6% νμ, 5.86% ν̄μ, 0.52% νe, 0.05% ν̄e |
| Peak νμ energy | ~800 MeV |

---

## Current Configuration (as of 2026)

- **Water volume:** 26 tons, gadolinium-loaded (Gd-water)
- **Photon detectors:** Three Gen-I LAPPDs (wall, downstream of beam) + 132 conventional PMTs
- **Muon Range Detector (MRD):** Downstream of beam; tracks exiting muons
- **Front Muon Veto (FMV):** Rejects muons from upstream neutrino interactions
- **SuperSANDI:** Nylon vessel inside the tank; being filled with Gd-loaded water-based liquid scintillator (WbLS) for hybrid Cherenkov/scintillation running (fall 2026)
- **Phase:** Transitioning from R&D/operations to physics data-taking and analysis

## Key R&D Milestones Achieved

- **2020:** First observation of neutrino-induced neutrons in Gd-loaded water
- **2021:** Neutron multiplicity data-taking period used in Nieslony 2022 analysis (runs 2421–2873, Jan–Jun 2021)
- **2022 (March):** First LAPPD deployment
- **2023:** First published observations of beam neutrino interactions in water-based liquid scintillator (WbLS)
- **2025:** Major PMT repair campaign (Sweeney); tilted PMT holders replaced, detector geometry restored
- **2026:** First observation of beam neutrino interactions in Gd-loaded water using LAPPDs (ISU-led publication)
- **2026:** SuperSANDI nylon vessel installation (Sweeney); hybrid C/S running anticipated fall 2026

## Detector Geometry Challenge

The ANNIE tank's small form factor — dimensions comparable to the size of its photosensors and structural elements — makes conventional water-Cherenkov reconstruction difficult. Photodetector acceptance and shadowing vary substantially across track topologies. This motivates the ISU group's development of the LitePath GPU ray-tracer, which explicitly models all structural detail.

## Collaborating Institutions

- **Iowa State University** (PI: Wetstein, PI: Weinstein) — lead institution; LAPPD systems, reconstruction, physics analysis
- **Florida State University** — Gen-II LAPPD purchase; neutron multiplicity analysis framework
- **Rutgers University** — CC0pi cross-section measurement (Rutgers-led)
- **Fermilab** — site host; engineer supporting Gen-II LAPPD electronics

*[Add full collaboration list]*

## Key References

- ANNIE Collaboration publications: *[add]*
- First LAPPD observation paper (2026): *[add]*
- WbLS paper (2023): *[add]*
- Nieslony 2022 dissertation: *Towards a neutron multiplicity measurement with ANNIE* — primary source for Phase II detector configuration
