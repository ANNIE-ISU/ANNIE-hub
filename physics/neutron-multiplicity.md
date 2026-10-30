---
title: "Neutron Multiplicity in ANNIE"
domain: physics
tags: [neutrons, neutrino-interactions, neutron-multiplicity, gadolinium, final-state-interactions]
links_to:
  - physics/neutrino-interactions.md
  - physics/overview.md
  - software/simulation-framework.md
  - detector/water-tank.md
  - detector/forward-muon-veto.md
  - analysis/neutron-multiplicity-nieslony2022.md
last_updated: "2026-10-30"
sources:
  - "Nieslony, Michael T. Towards a neutron multiplicity measurement with ANNIE. Univ. Mainz PhD Dissertation, 2022."
---

# Neutron Multiplicity in ANNIE

## Physics motivation

The **neutron multiplicity** (or *neutron yield*) is the number of neutrons produced per neutrino interaction. It is a sensitive probe of two related but distinct physics processes:

- **Final State Interactions (FSIs)** within the struck nucleus — intranuclear rescattering can knock out additional neutrons even in quasi-elastic events
- **Secondary hadronic interactions** in the water target — pions and other hadrons produced in resonant or deep inelastic events can rescatter on oxygen nuclei, producing additional neutrons

Because neutron multiplicity correlates with the inelasticity of the interaction, it can help tag interaction topology (CC-QE vs. CC-RES vs. CC-DIS) and reduce neutrino energy reconstruction bias. It is also a direct test of hadronic models in neutrino event generators (GENIE, NuWro, NEUT), which are known to have deficiencies in this regime.

Beyond neutrino physics, neutron yield measurements constrain backgrounds in searches for the Diffuse Supernova Neutrino Background (DSNB) and proton decay, both of which rely on accurate modeling of the hadronic final state.

## Why ANNIE is well-suited to this measurement

Three detector properties combine to make ANNIE competitive for neutron multiplicity:

**Gadolinium-doped water.** Neutrons thermalize and capture on Gd nuclei within ~30 μs, producing a cascade of ~4–5 gamma rays with total energy ~8 MeV. This is a far more detectable signal than capture on hydrogen (single 2.2 MeV gamma). The Gd loading gives ANNIE sensitivity to individual neutrons at the few-photoelectron level.

**Extended DAQ window.** ANNIE records a 70 μs acquisition window following each beam trigger — long enough to capture the full neutron capture time distribution — alongside the standard 2 μs prompt window for the muon.

**MRD + FMV for muon tagging.** The Muon Range Detector provides stopping muon identification and track reconstruction, enabling event-by-event muon energy and angle measurement. The Forward Muon Veto tags incoming dirt muons. Together they define a clean neutrino candidate sample and enable the neutron yield to be studied as a function of muon kinematics.

## General detector-level signal characteristics

A neutron capture event in ANNIE's Gd-doped water produces:
- **Total charge:** 10–40 p.e. across the PMT array
- **PMT multiplicity:** 10–20 hit PMTs
- **Capture time:** exponential distribution with τ ≈ 30 μs (theoretical); observed τ consistent with this in data
- **Background:** afterpulsing (concentrated at 6–10 μs after prompt event) and uncorrelated dark noise hits

The dominant selection challenge is discriminating true neutron captures from these backgrounds at low charge. The **Charge Balance** parameter q_CB (a measure of how asymmetrically charge is distributed across PMTs) is a key discriminating variable alongside total cluster charge.

## Theoretical expectation by interaction channel

GENIE (v2.12.0) predicts the following probability of producing at least one primary neutron (from the neutrino-nucleus interaction and nuclear FSIs):

| Channel | P(≥1 primary neutron) |
|---|---|
| CC-QE | ~34% (all from nuclear FSI; QE kinematics yield a proton) |
| CC-RES | ~52% |
| CC-DIS | ~68% |

Including secondary neutrons from hadronic rescattering in Geant4 extends the total distribution: while primary neutrons reach at most ~8, total (primary + secondary) multiplicity can reach ≥30 in rare cases. Secondary processes also substantially increase the probability of observing at least one neutron in CC-QE events.

These generator predictions are among the primary things the ANNIE measurement is designed to test.

## Analyses

| Analysis | Status | Dataset | Key result |
|---|---|---|---|
| [Nieslony 2022](../analysis/neutron-multiplicity-nieslony2022.md) | Published (dissertation) | 2021 beam year, Runs 2506–2630 | n̄_data = 0.272 ± 0.010 vs. n̄_MC = 0.515 ± 0.007; data deficit >5σ, no kinematic dependence |
| Current collaboration analysis | In development | — | — |

