# Veto Systems: FMV and MRD

**Domain:** Detector  
**Links to:** [Detector Overview](overview.md) · [Trigger System](trigger-system.md)  
**Last updated:** 2026-10-28

---
sources:
  - author: "He"
    year: 2024
    type: dissertation
    institution: "UC Davis"
    advisor: "Svoboda"
    notes: "Chapter 3 §§3.2.5–3.2.6 — FMV and MRD hardware, geometry, DAQ"
  - author: "Nieslony"
    year: 2022
    type: dissertation
    institution: "Univ. Mainz"
    notes: "Chapter 3 — MRD geometry; Chapter 4 — MRD and FMV efficiency measurements"
---

## Front Muon Veto (FMV)

The FMV is located **upstream** of the water tank. It identifies muons produced by neutrino interactions in the surrounding rock ("dirt muons") before they enter the tank, preventing them from contaminating the neutrino interaction sample.

### Hardware (He 2024, Ch. 3)

| Parameter | Value |
|-----------|-------|
| Position | Upstream of water tank |
| Layers | 2 |
| Paddles per layer | 13 |
| Total paddles | 26 |
| Paddle dimensions | 322 cm × 31 cm × 2.5 cm |
| Scintillator type | Plastic (recycled from CDF experiment) |
| PMT | EMI-9815 with tapered glass light guide |
| High voltage | 2,000–2,500 V |
| Layer offset | ~0.5 cm between layers |
| Total footprint | ~3.2 m wide × ~4 m tall |

Layers are horizontally offset by ~0.5 cm to prevent neutrinos from passing through gaps in both layers simultaneously.

### Logic

- OR of two layers: a hit in **either** layer flags the event as a potential dirt muon
- In He 2024 analysis: any FMV activity causes the event to be **rejected** from the muon candidate sample

### Efficiency

From Nieslony 2022 (Chapter 4), measured in-situ using through-going cosmic muon tracks:

| Metric | Value |
|--------|-------|
| FMV tagging efficiency | 95.6 ± 1.6% |
| Average miss probability | 4.4 ± 1.6% |

---

## Muon Range Detector (MRD)

The MRD is located **downstream** of the water tank. Its primary function is to range out muons from neutrino interactions, providing a measurement of muon momentum (via range), direction (via track reconstruction), and a second interaction vertex to confirm the muon candidate originated in the water tank.

The MRD is a legacy of the **SciBooNE experiment**, re-used and re-instrumented for ANNIE.

### Hardware (He 2024, Ch. 3)

| Parameter | Value |
|-----------|-------|
| Position | Downstream of water tank |
| Total footprint | ~3.5 m wide × ~4 m tall |
| Frame | Aluminum support frame |
| Iron absorber slabs | 11 slabs, each 274 × 305 × 5 cm, density 7.841 g/cm³ |
| Scintillator paddles | 306 total |
| Paddle layers | 11 total: 6 horizontal + 5 vertical |
| Readout | Photomultiplier tubes on scintillator paddles |
| Efficiency | 80–95% per paddle (in-situ measured) |

### DAQ (He 2024, Ch. 3)

| Component | Model |
|-----------|-------|
| Discriminators | LeCroy 4413 CAMAC |
| TDCs | LeCroy 3377 |
| HV supply | LeCroy 1440 |

### Track Reconstruction

The MRD uses a **cellular automation** algorithm for track reconstruction (K2K/T2K heritage, adapted from NEUT). The algorithm reconstructs 3D tracks from hit pattern in the scintillator layers.

| Metric | Value |
|--------|-------|
| Angular resolution | σ ≈ 0.16 rad |
| Track efficiency | 80–95% |

For events where the muon scatters significantly in the iron, an **effective track length** correction is applied: using the reconstructed track angle θ_MRD, the effective path length through each iron slab is adjusted as L_eff = L × cos θ_MRD.

### Role in Event Selection

In the He 2024 CC-0π analysis, the MRD must have **exactly one stopping track** — the muon must range out inside the MRD (not exit), so its full momentum can be determined. The track is also extended backward to verify it points to the tank interaction region.
