# Simulation Framework

**Domain:** Software  
**Links to:** [Software Overview](overview.md) · [Neutrino Interactions](../physics/neutrino-interactions.md)  
**Last updated:** 2026-10-28

---
sources:
  - author: "Nieslony"
    year: 2022
    type: dissertation
    institution: "Univ. Mainz"
    notes: "Chapter 5 primary source for simulation chain, geometry, modifications, and validation"
---

## What This Is

ANNIE uses a two-stage Monte Carlo simulation chain to model neutrino interactions, particle propagation, and detector response.

---

## Simulation Chain

```
BNB flux files  ──►  GENIE (gevgen_fnal)  ──►  WCSim (Geant4)  ──►  Beam sample
                    [neutrino interaction]    [detector propagation
                                              + electronics response]
```

1. **Neutrino flux:** BNB flux files from MiniBooNE/Fermilab
2. **Interaction generator:** GENIE `gevgen_fnal` — generates νμ CC/NC interactions; outputs final-state particle kinematics
3. **Particle propagation:** WCSim (Geant4-based) — propagates all final-state particles, generates Cherenkov photons, models electronics response
4. **Digitization:** PMT charge and timing response modelled within WCSim; includes realistic quantum efficiency curves and single-PE charge functions
5. **Reconstruction:** Same algorithms applied to MC as to data

An independent simulation environment, **rat-pac** (written by Vincent Fischer), also exists and is mainly used for neutron studies and future WbLS phase planning. WCSim is the primary framework for Phase II.

---

## WCSim: Detector Simulation

WCSim is a Geant4-based Water Cherenkov simulation framework originally developed for Hyper-Kamiokande and Super-Kamiokande studies. The ANNIE-specific version is the `annie` branch of the `ANNIEsoft` fork of WCSim, implemented by Marcus O'Flaherty.

### Physics List

WCSim uses the `FTFP_BERT_HP` physics list. The **High Precision (HP)** component is essential for accurately modelling hadronic transport and neutron capture processes at low energies (Ekin < 20 MeV). This is critical for ANNIE's Gd neutron-capture signal.

### Detector Geometry

- Water tank, Inner Structure, FMV, and MRD are all implemented
- Multiple PMT types (different sizes and quantum efficiencies) modelled individually
- LAPPDs implemented as a separate photosensor type; up to 3 positions per mailbox slot, 8 slots → maximum 24 LAPPD positions
- Inner Structure geometry derived from original CAD drawings (gdml file)
- Scintillator paddles (FMV and MRD) configured with detection efficiency of 95%
- Coordinate origin: centre of FMV; tank centre at (0, −14.45, 168.1) cm

---

## Simulation Modifications (Nieslony 2022)

The following improvements were made to the baseline WCSim geometry in the course of the Phase II analysis:

- PMT holder structures added to geometry for accurate shadowing
- PMT positions updated from a dedicated laser scan campaign (replacing design positions)
- Realistic quantum efficiency curves introduced for each PMT type
- Single-PE charge functions added per PMT type
- Reflectivities of inner surfaces tuned to data (particularly upstream wall reflectivity increased)
- Charge scale correction factor applied: data charge ~37.5% higher than initial MC prediction (fconv ≈ 1.375, derived from commissioning data plateau comparison)

---

## Validation Against Calibration Samples

The tuned simulation was validated against three independent calibration datasets. All χ²/n.d.f. values reflect agreement between data and MC distributions of relevant PMT observables.

| Calibration sample | χ²/n.d.f. | Notes |
|--------------------|-----------|-------|
| Michel electrons (μ→e decay) | 0.95 | Energy scale, vertex resolution |
| Through-going muons | 1.05 | Reflectivity, geometric acceptance |
| AmBe neutron source | 2.12 | Neutron capture, Gd γ-cascade modelling |

The AmBe χ²/n.d.f. of 2.12 indicates residual discrepancies in the neutron capture simulation, which is one of the systematic uncertainties in the neutron multiplicity measurement.

---

## GENIE: Interaction Generator

See [Neutrino Interactions](../physics/neutrino-interactions.md) for the full list of physics models used in GENIE.

Key GENIE inputs for ANNIE:
- BNB flux files (same flux parametrisation as MiniBooNE)
- ANNIE detector geometry in gdml format (includes surrounding rock for dirt event simulation)
- Interactions restricted to water volume or extended to surrounding rock (configurable)

---

## Running Simulation Jobs

*[Fill in: how to submit simulation jobs, where outputs go, how much CPU is typically needed, job submission scripts.]*
