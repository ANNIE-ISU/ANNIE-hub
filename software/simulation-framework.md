# Simulation Framework

**Domain:** Software  
**Links to:** [Software Overview](overview.md) · [Neutrino Interactions](../physics/neutrino-interactions.md)  
**Last updated:** 2026-10-26

---

## What This Is

ANNIE uses a Monte Carlo simulation chain to model neutrino interactions, particle propagation, and detector response.

## Simulation Chain

1. **Neutrino flux:** BNB flux files from MiniBooNE/Fermilab
2. **Interaction generator:** GENIE *[version]* — generates neutrino interactions in the water target
3. **Particle propagation:** WCSim (Geant4-based) — propagates final-state particles, generates Cherenkov photons, simulates detector geometry
4. **Digitization:** *[fill in: how photon hits are converted to waveforms/hits]*
5. **Reconstruction:** *[fill in: what reco is applied to MC]*

## Geometry

The ANNIE detector geometry is defined in WCSim. *[Fill in: geometry file location, current version, known geometry issues.]*

## Validation

*[Fill in: how MC is validated against data, key comparison plots, any known MC/data discrepancies.]*

## Running Simulation Jobs

*[Fill in: how to submit simulation jobs, where outputs go, how much CPU is typically needed.]*
