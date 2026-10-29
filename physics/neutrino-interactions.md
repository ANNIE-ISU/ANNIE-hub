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
    notes: "Chapter 6 primary source for neutron multiplicity results; Chapter 5 for GENIE interaction models"
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

ANNIE uses the **GENIE** (Generates Events for Neutrino Interaction Experiments) event generator framework, run via the `gevgen_fnal` special generator for BNB beam simulations at Fermilab. GENIE is the standard generator for Fermilab-based beam neutrino experiments.

### Cross-section models in GENIE (as used in Nieslony 2022)

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

Neutrons are produced in neutrino-nucleus interactions via:
- Neutrons present in the initial nuclear state that are knocked out during the interaction
- Neutrons produced in hadronic cascades (pion absorption, etc.)
- Final State Interactions (FSIs) inside the nucleus before escape

The neutron multiplicity (number of neutrons per neutrino interaction) is the primary physics observable of ANNIE Phase II.

---

## Measured Neutron Multiplicity (Nieslony 2022, 2021 beam data)

Results from the first Phase II neutron multiplicity measurement using runs 2421–2873 (January–June 2021). Neutrons are identified via their capture signature on Gd (γ cascade, ~8 MeV total).

| Sample | Data | Monte Carlo (GENIE+WCSim) |
|--------|------|--------------------------|
| **Inclusive** (all CC events) | **0.272 ± 0.010** | **0.515 ± 0.007** |
| **Fiducial Volume** (FV-selected) | **0.287 ± 0.044** | **0.627 ± 0.031** |

**Key finding:** The GENIE + WCSim simulation overpredicts the measured neutron multiplicity by approximately a factor of **2** in both samples. This significant discrepancy motivates further measurements and generator tuning.

*Note: Uncertainties are statistical; systematic uncertainties are under study. The data values are consistent with each other across inclusive and FV selections, validating the fiducialisation procedure.*

---

## Key Observables

ANNIE reconstructs:
- **Muon kinematics:** Vertex from tank activity; muon energy via MRD range; muon direction from MRD track
- **Neutron capture time:** Delayed coincidence window up to ~70 μs post-beam; captures on Gd identified by γ cascade
- **Neutron multiplicity:** Count of observed neutron captures per neutrino interaction event
- **Fiducial volume:** Events with reconstructed vertex inside a defined inner region of the tank
