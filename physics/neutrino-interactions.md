# Neutrino Interactions

**Domain:** Physics  
**Links to:** [Physics Overview](overview.md) · [Backgrounds](backgrounds.md) · [Simulation Framework](../software/simulation-framework.md)  
**Last updated:** 2026-10-28

---
sources:
  - author: "Nieslony"
    year: 2022
    type: dissertation
    institution: "Univ. Mainz"
    notes: "Chapter 6 primary source for first neutron multiplicity results (2021 data); Chapter 5 for GENIE interaction models"
  - author: "He"
    year: 2024
    type: dissertation
    institution: "UC Davis"
    advisor: "Svoboda"
    notes: "Chapter 6 primary source for He 2024 neutron multiplicity analysis; supersedes Nieslony 2022 on methodology and results where they overlap"
---

## Relevant Interaction Channels

At BNB energies (~few hundred MeV to ~2 GeV), the dominant channels in water are:

- **Quasi-elastic scattering (CCQE):** νμ + n → μ⁻ + p
- **Resonant pion production (RES):** νμ + N → μ⁻ + N' + π
- **Deep inelastic scattering (DIS):** at higher energies
- **Coherent pion production**

Neutron yields differ significantly between these channels, which is why measuring multiplicity constrains the interaction mix.

---

## Interaction Generator: GENIE

ANNIE uses the **GENIE** (Generates Events for Neutrino Interaction Experiments) event generator framework, run via the `gevgen_fnal` special generator for BNB beam simulations at Fermilab.

### Cross-section models in GENIE (Nieslony 2022)

| Process | Model |
|---------|-------|
| CCQE | Llewellyn-Smith, Smith-Moniz, or Nieves models |
| Neutral Current Elastic | Ahrens model |
| Baryonic resonances | Rein-Sehgal model |
| Inelastic scattering | Bodek-Yang model |
| Nuclear structure | Relativistic Fermi gas; Bodek-Ritchie parametrization for short-range correlations |
| Hadronization | PYTHIA/JETSET (high energy) + Koba-Nielsen-Olesen scaling (low energy) |
| Final State Interactions | Included (hadronic cascades in nucleus) |

BNB flux is provided to GENIE via BNB flux files (consistent with MiniBooNE).

---

## Neutron Production in νμ CC Interactions

Neutrons are produced via: nuclear knock-out from the initial state, hadronic cascades (pion absorption, etc.), and Final State Interactions (FSIs) inside the nucleus before escape. The neutron multiplicity (number of neutrons per neutrino interaction) is the primary physics observable of ANNIE Phase II.

---

## Nieslony 2022: First Neutron Multiplicity Measurement

Runs 2421–2873 (January–June 2021). Neutrons identified via Gd capture γ cascade (~8 MeV).

| Sample | Data | MC (GENIE+WCSim) |
|--------|------|-----------------|
| **Inclusive** | **0.272 ± 0.010** | **0.515 ± 0.007** |
| **Fiducial Volume** | **0.287 ± 0.044** | **0.627 ± 0.031** |

Simulation overpredicts by ~factor 2 in both samples.

---

## He 2024: Neutron Multiplicity Analysis (CC-0π νμ)

He 2024 (UC Davis, advisor Svoboda) presents the full Phase II analysis using runs 2506–2643 (2021) and 3774–4188 (2023). Very few 2023 events passed selection due to known MRD multi-track issues.

### Muon Candidate Selection (7 cuts)

1. **BEAM trigger** (trigger word 5)
2. Tank cluster within **2000 ns** of beam spill start
3. **Largest charge** cluster selected if multiple
4. **No FMV hits** (rejects dirt muons)
5. **Exactly one MRD track stopping in MRD**
6. Backward track extension **crosses 1 m³ sphere at tank centre**
7. Tank-MRD timing: **700–800 ns** (data) / **0 ± 50 ns** (MC)

### Vertex Reconstruction (He Development)

Ring-edge Cherenkov method using PMTs only:

- For each PMT i, track segment: aᵢ = −(μ̂ × R⃗ᵢ)/tan θ_C + (μ̂ · R⃗ᵢ)
- Photon density η = (total PE in ring frustum) / (f_total × Δx); η drops at ring edge
- Fraction of PMT i photocathode overlapping ring: f_i = (A_PMT,i / 2)(1 + cos θᵢ) / ΔA_ring
- **Manual fit:** mean −12 cm bias, σ = 30 cm, RMS = 32 cm
- **RNN model** (Tianqi Zhang): mean 0.99 cm bias, σ = 16.3 cm — used for majority of events
- **Vertex** = tank exit point moved back by fitted track length
- **Spatial resolution:** ~60 cm at 80% (He method); ~40 cm with 5 LAPPDs in simulation

### Fiducial Volume

Half-cylinder, radius 1 m, height 1 m, ~2.5 tonnes — offset upstream from tank centre to maximise neutron detection acceptance.

### Muon Energy Reconstruction

- Water: dE/dx ≈ 2 MeV/cm; MRD iron: dE/dx ≈ 11 MeV/cm (average)
- Iterative Bethe-Bloch stepping in 2 cm increments until kinetic energy < 20 MeV
- Energy resolution: σ ≈ 66 MeV (mean bias −15 MeV); ~10% at 800 MeV

### Q² Reconstruction

```
E_rec^ν = [2(M−E_b)E_μ − (E_b² − 2ME_b + m²_μ + ΔM²)] / [2(M − E_b − E_μ + |p_μ|cos θ_μ)]
Q² = 2 E_rec^ν (E_μ − p_μ cos θ_μ) − M²_μ
```

### Neutron Candidate Selection (Delayed Window)

Beam trigger opens 2 μs prompt window; ≥5 PE in any PMT extends to 70 μs. Delayed window (10–70 μs used; 2–10 μs excluded for after-pulsing).

| Cut | Value | Reason |
|-----|-------|--------|
| t_cluster | > 10 μs | Muon after-pulsing exclusion |
| Q_tot | < 120 PE | Remove large non-neutron clusters |
| CB | < 0.4 | Reject noise/isolated hits |
| CB | < 0.5 − Q_tot/300 PE | Combined CB cut |

- **Measured capture time constant:** τ = 33 ± 2 μs (expected ~30 μs in Gd-water)
- **Time-cut efficiency:** ε_t ≈ 62% (~38% of captures missed)

### Neutron Detection Efficiency (AmBe)

Averaged over Ports 1 and 5 (upstream of FV), heights −50 to +50 cm:

| | Data | MC |
|-|------|----|
| **Efficiency** | **65%** | **76%** |

### Neutron Background

Phase I measurement: **0.02 neutrons/beam-spill/m³** in FV. Applied as overall correction.

---

## He 2024 Final Results (Table 6.1)

| Selection | Data n̄ | MC n̄ |
|-----------|--------|-------|
| Stopping MRD track | 0.294 ± 0.015(stat) ± 0.18(sys) | 0.442 ± 0.007(stat) ± 0.27(sys) |
| + FV cut | 0.294 ± 0.039(stat) ± 0.18(sys) | 0.449 ± 0.018(stat) ± 0.27(sys) |
| **+ n efficiency** | **0.452 ± 0.039(stat) ± 0.27(sys)** | **0.582 ± 0.018(stat) ± 0.38(sys)** |

*Dominant systematic: timing cut efficiency (ε_t ≈ 62%).*

**Neutron containment check:** upstream vs. downstream FV comparison gives σ_sys,data = 0.002 (negligible). Combined FV + efficiency: n̄_data = 0.450 ± 0.022(stat), n̄_MC = 0.583 ± 0.009(stat).

**Interpretation:** Data/MC ≈ 0.78. ANNIE data is consistently below Super-K and SNO measurements at similar energies. Discrepancy not understood; nuclear interaction models require further investigation.

---

## Key Observables

- **Muon vertex:** Ring-edge Cherenkov method (He 2024) or track-back; resolution ~60 cm with PMTs only
- **Muon energy:** MRD range + Bethe-Bloch iteration; resolution ~10% at 800 MeV
- **Muon direction:** MRD cellular automation track reconstruction (K2K/T2K heritage)
- **Q²:** Reconstructed from quasi-elastic kinematics
- **Neutron multiplicity:** Count of delayed Gd-capture clusters, efficiency-corrected
- **Neutron capture time:** τ ≈ 33 μs measured (30 μs expected) in Gd-loaded water
