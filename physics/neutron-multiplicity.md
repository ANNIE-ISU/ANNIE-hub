---
title: "ANNIE Neutron Multiplicity Measurement"
domain: physics
tags: [neutrons, neutrino-interactions, neutron-multiplicity, analysis, gadolinium, muon-reconstruction, fiducial-volume]
links_to:
  - physics/neutrino-interactions.md
  - physics/overview.md
  - software/simulation-framework.md
  - detector/water-tank.md
  - detector/muon-range-detector.md
  - detector/forward-muon-veto.md
last_updated: "2026-10-30"
sources:
  - "Nieslony, Michael T. Towards a neutron multiplicity measurement with ANNIE. Univ. Mainz PhD Dissertation, 2022. Ch. 6."
---

# ANNIE Neutron Multiplicity Measurement

## Overview

The **neutron multiplicity** (or *neutron yield*) — the number of neutrons produced per neutrino interaction — is a key observable for ANNIE Phase II. It encodes information about the inelasticity of the neutrino-nucleus interaction and the hadronic final state, and is directly sensitive to both Final State Interactions (FSIs) within the struck nucleus and secondary neutron production from hadronic rescattering in the water target. Accurate modeling of neutron multiplicity is needed to constrain backgrounds in DSNB and proton decay searches, making ANNIE's Gadolinium-loaded water target and extended DAQ window well-suited to this measurement.

The analysis described here uses 2021 beam year data (Runs 2506–2630, 21% of adequate beam runs) and reconstructs both the prompt muon candidate and delayed neutron candidates in the same event. The core finding is that data consistently shows **fewer neutrons than Monte Carlo predicts**, with average observed multiplicities roughly half the MC expectation, independently of muon energy or angle.

---

## Data Sample and Neutrino Candidate Selection

### Run period

The analysis uses BNB neutrino beam data from **Runs 2506–2630** (2021 beam year). Beam spills produce a 1.6 μs window of neutrino activity inside a 2 μs DAQ acquisition window, after which an extended 70 μs window captures delayed neutron candidates. Beam and off-beam triggers each run at 3–5 Hz.

Subsystem timing offsets established during commissioning:

| Subsystem pair | Offset |
|---|---|
| Δt_MRD,PMT | ≈ 750 ns |
| Δt_FMV,PMT | ≈ 800–815 ns (stable ±10 ns) |
| Δt_MRD,FMV | ≈ 50 ns |

### Event selection cuts (Table 6.1)

Sequential cuts reduce the full cluster sample to a clean neutrino candidate sample:

| Cut | Events remaining |
|---|---|
| All clusters (any trigger) | 10,693,314 |
| MRD coincidence (≥200 p.e. tank charge, 700 < Δt_MRD,PMT < 800 ns) | 24,146 |
| No FMV veto (750 < Δt_FMV,PMT < 850 ns) | 16,004 |
| Good beam (POT/spill, horn current within spec) | 15,723 |
| Extended window available (70 μs) | 10,932 |

The MRD coincidence requirement ensures a stopping muon track is present. The FMV veto removes events where a charged particle entered through the tank wall (dirt events). Beam quality cuts on POT/spill and horn current exclude unstable beam conditions.

---

## Prompt Event Analysis: Muon Reconstruction

### Energy reconstruction

Muon energy is reconstructed from three components corresponding to different detector regions.

**Tank component** — estimated from total observed charge Q using dE/dx ≈ 1.992 MeV/cm for MIP muons:

    E_μ,tank = α · Q                        (Eq. 6.3)

**Outer volume component** — the muon traverses an optically isolated outer region of the tank before the MRD; invisible to PMTs but reconstructible from the MRD track:

    E_μ,outer = β · d_outer                 (Eq. 6.4)

where β ≈ 1.992 MeV/cm and d_outer = |r_tank,exit − r_PMT,exit| is extrapolated from the MRD track.

**MRD energy loss** — E_loss,MRD is determined from muon range and stopping position in the MRD.

**Total reconstructed muon energy:**

    E_μ,reco = α·Q + β·d_outer + E_loss,MRD + c    (Eq. 6.6)

Fit of E_tank vs. Q from simulation:

    α = (0.08534 ± 0.00169) MeV/p.e.
    c = (87.3 ± 3.9) MeV

Consistent with the independent determination α′ = (0.0833 ± 0.08) MeV/p.e. from Pershing (2021). Simulation energy resolution: μ_energy ≈ 7 MeV, σ_energy ≈ 117 MeV; high-energy tail from pion events.

### Vertex reconstruction

The interaction vertex is extrapolated along the MRD track direction d̂_MRD back into the water volume:

    r_vertex = r_PMT,exit − (α/β)·Q·d̂_MRD        (Eq. 6.8)

Position resolution from simulation: σ_xi ≈ 35 cm for all three coordinates. A tail at Δz ≈ −1 m reflects events reconstructed too far upstream.

### Fiducial Volume (FV) definition

Events are restricted to the upstream region of the tank to minimize sky-shine neutron contamination and maximize acceptance for beam-forward neutrons:

    ρ = √(x² + z²) < 1.0 m
    |y| < 0.5 m
    z < 0.0 m

Muon energy (peak ~800–1000 MeV) and angular distributions (mostly cos θ_μ ≈ 1) agree well between data and simulation.

---

## Delayed Event Analysis: Neutron Counting

### Expected neutron yield (§6.4.1)

Neutron production is simulated using GENIE (v2.12.0) for the neutrino-nucleus interaction and Geant4/WCSim for propagation and secondary interactions. Two categories:

- **Primary neutrons**: from the neutrino-nucleus interaction directly (nuclear breakup, FSIs within oxygen nucleus)
- **Secondary neutrons**: from hadronic rescattering of final-state particles in the water target

GENIE-predicted probability of ≥1 primary neutron by interaction channel:

| Channel | P(n ≥ 1) | Comment |
|---|---|---|
| CC-QE | ~34% | From nuclear FSI only; QE kinematics yield a proton |
| CC-RES | ~52% | Resonance production more likely to break nucleus |
| CC-DIS | ~68% | Highest inelasticity → most neutrons |

Including secondary Geant4 interactions extends the total multiplicity distribution to ≥30 in rare cases (vs. maximum ~8 for primary only), and significantly increases the fraction of events with at least one neutron.

### Neutron candidate selection cuts (§6.4.2)

Gd-capture signals are faint: **10–40 p.e. across 10–20 PMTs** (vs. ~130 PMT hits for the prompt muon). Must be discriminated from afterpulsing and dark noise.

Selection criteria from the AmBe calibration campaign:

    q_total < 120 p.e.
    q_CB < 0.4
    q_CB < 0.5 − q_total / 300 p.e.

where q_CB is the Charge Balance parameter (see software/simulation-framework.md). Events in the lower-left region of the q_CB vs. q_total plane are selected as neutron candidates.

**Search window: 10–70 μs** after the beam trigger. The 6–10 μs interval is excluded due to enhanced afterpulsing (visible as increased PMT activity in typical extended-window events).

### Properties of selected neutron candidates (§6.4.3)

**Time spectrum** of 1769 candidates from beam data fitted with:

    f(t) = c + A · exp(−t/τ)                       (Eq. 6.13)

Best-fit parameters:

    c = (14 ± 12) counts/2400 ns
    τ = (29 ± 7) μs

Consistent with theoretical τ_theo = 30 μs for Gd capture, validating the selection.

**Charge spectrum**: peaks between 10–30 p.e., decreasing slope to ~70 p.e., in good agreement between data and MC.

---

## Integrated Neutron Multiplicity Results (§6.4.4)

Neutron candidates are counted per neutrino candidate event and compared with MC predictions.

### Stopping MRD track sample (all neutrino candidates)

Incompatibility with MC prediction: **>5σ**

    χ²/n.d.f. = 254.8/9,  p < 1×10⁻⁷

Average neutron yields:

    n̄_MC   = 0.515 ± 0.007 (stat)
    n̄_data = 0.272 ± 0.010 (stat)

Events with n_neutron ≥ 2 are suppressed by more than a factor of 2 in data vs. simulation.

### Fiducial Volume sample (FV-origin muons)

Incompatibility: **2.33σ**

    χ²/n.d.f. = 20.5/9,  p = 0.015

Average neutron yields:

    n̄_FV,MC   = 0.627 ± 0.031 (stat)
    n̄_FV,data = 0.287 ± 0.044 (stat)

No events with 3 or 4 neutrons observed in the FV data due to limited statistics. The less significant tension (vs. MRD-only sample) is partly because systematic uncertainties (not yet included) will reduce the tension, and the FV sample is smaller.

The pattern — models overpredict neutron yield at all multiplicities ≥2 — suggests hadronic interaction models in Geant4 overestimate secondary neutron production, and/or GENIE produces too many neutrons from FSIs.

---

## Neutron Multiplicity vs. Muon Kinematics (§6.5)

### Method

The neutron multiplicity is analyzed as a 2D function of reconstructed muon energy E_μ and interaction angle cos(θ_μ), using a forward-folding approach (detector-level comparison, not unfolded to true neutrino kinematics).

Average multiplicity and uncertainty per energy or angle bin i:

    μ_i = (1/N_total,i) Σ_j j · n_ij
    σ_i = √[ 1/(N_total,i − 1) · Σ_j n_ij(j − μ_i)² ]

### Energy dependence

Energy range: 400–1200 MeV (stopping MRD track), 600–1200 MeV (FV). **No significant energy dependence** is found in data or MC for either sample. Average measured multiplicity ≈ 0.25 neutrons/event across all energy bins; MC predicts ≈ 0.5 neutrons/event, consistently higher.

### Angular dependence

Angular range: cos(θ_μ) ∈ [0.7, 1.0] (MRD track), [0.8, 1.0] (FV). **No significant angular dependence** found. The deficit of ≈ 0.25 neutrons (data) vs. ≈ 0.5 (MC) is approximately uniform.

### Conclusion

The neutron deficit is **not localized** to any particular kinematic region. The overestimation by models appears to be a global property across muon energies 400–1200 MeV and angles cos(θ_μ) ∈ [0.7, 1.0], suggesting a systematic issue with hadronic model neutron production rather than a topology-specific mismodeling.

---

## Systematic Uncertainties Outlook (§6.6)

Systematic uncertainties are not yet included in the published results but are under active investigation. Key sources:

### Muon reconstruction uncertainty

- σ_E ≈ 120 MeV, σ_θ ≈ 0.17 rad
- Creates bin migration in the 2D kinematic distributions
- Migration matrices show clean linear correlation between true and reconstructed variables
- Impact expected small given the flat kinematic dependence of n̄

### Fiducial Volume selection

- Vertex resolution (~35 cm) allows events to migrate across the FV boundary
- Since n̄ for FV sample is compatible with the full-tank result, estimated negligible currently
- Will become important with increased statistics

### FMV inefficiency — dirt muon contamination

The FMV efficiency ε_FMV ≈ 90%, so ~10% of dirt events enter the neutrino candidate sample. From flagged veto events:

    n̄_dirt = 0.113 ± 0.010  (N_dirt,det = 1715)
    n̄_ν,det = 0.272 ± 0.011  (N_ν,det = 3564)

FMV-corrected true neutrino neutron yield:

    n̄_ν = (0.281 ± 0.012)                          (Eq. 6.16)

    σ_sys,FMV = 0.009

For the FV sample, this systematic is negligible due to the strong vertex constraint.

### Neutron detection efficiency

Mapped as a function of position using AmBe source at 5 ports × 5 heights (25 positions):

- Ports 1 and 5: MC ≈ 10% higher efficiency than data
- Ports 2, 3, 4: data/MC in good agreement
- Average relative difference: **~9%** over all positions

Effect of 9% efficiency correction on MC:

    n̄_MC(nominal) = 0.515 ± 0.007
    n̄_MC(reduced) = 0.469 ± 0.007

    σ_sys,eff ≈ 0.046  (~9% relative)

Since MC consistently overestimates efficiency, applying corrections will introduce asymmetric error bars and slightly reduce the tension between data and simulation.

### Future improvements

- Ring-counting (CC-0π topology isolation) to more directly probe FSIs without CC-RES/DIS contamination
- Position-dependent efficiency correction factors covering the full 3D tank volume
- Larger datasets from 2022+ run periods
- Improved reconstruction (deep learning vertex and energy estimators)

---

## Summary Table

| Observable | Value |
|---|---|
| Final neutrino candidate sample | 10,932 events |
| Neutron candidates in beam data | 1,769 |
| Observed neutron capture time τ | (29 ± 7) μs |
| n̄_MC — stopping MRD track | 0.515 ± 0.007 |
| n̄_data — stopping MRD track | 0.272 ± 0.010 |
| Data/MC tension (stopping MRD track) | >5σ (χ²/n.d.f. = 254.8/9) |
| n̄_MC — FV sample | 0.627 ± 0.031 |
| n̄_data — FV sample | 0.287 ± 0.044 |
| Data/MC tension (FV sample) | 2.33σ (χ²/n.d.f. = 20.5/9) |
| FMV-corrected n̄_ν | 0.281 ± 0.012 |
| Neutron det. efficiency systematic | ~9% (σ_sys,eff ≈ 0.046) |
| Kinematic (E_μ, θ_μ) dependence | Not observed |
