---
title: "Neutron Multiplicity Analysis — Nieslony 2022"
domain: physics
analysis_status: "published-dissertation"
analysis_vintage: "Nieslony 2022"
run_range: "2506-2630"
tags: [neutrons, neutrino-interactions, neutron-multiplicity, analysis, gadolinium, muon-reconstruction, fiducial-volume]
links_to:
  - physics/neutron-multiplicity.md
  - physics/neutrino-interactions.md
  - software/simulation-framework.md
  - detector/water-tank.md
  - detector/muon-range-detector.md
  - detector/forward-muon-veto.md
last_updated: "2026-10-30"
sources:
  - "Nieslony, Michael T. Towards a neutron multiplicity measurement with ANNIE. Univ. Mainz PhD Dissertation, 2022. Ch. 6."
---

# Neutron Multiplicity Analysis — Nieslony 2022

This article documents the first ANNIE neutron multiplicity analysis, described in Ch. 6 of [Nieslony 2022]. It is specific to the 2021 beam year dataset (Runs 2506–2630) and the reconstruction and selection methods developed for that analysis. For general physics motivation and context, see [Neutron Multiplicity in ANNIE](../physics/neutron-multiplicity.md).

The core finding is that data consistently shows **fewer neutrons than Monte Carlo predicts**, with average observed multiplicities roughly half the MC expectation, independently of muon energy or angle.

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

The MRD coincidence requirement ensures a stopping muon track is present. The FMV veto removes events where a charged particle entered through the tank wall (dirt events). Beam quality cuts on protons-on-target per spill and horn current exclude unstable beam conditions.

---

## Prompt Event Analysis: Muon Reconstruction

### Energy reconstruction

Muon energy is reconstructed from three components corresponding to different regions of the detector:

**Tank component** — estimated from total observed charge Q in the PMT array using the minimum-ionizing energy loss (dE/dx ≈ 1.992 MeV/cm for MIP muons):

$$E_{\mu,\text{tank}} = \alpha \cdot Q \tag{6.3}$$

**Outer volume component** — the muon continues through an optically isolated outer region of the tank before entering the MRD; this energy is invisible to the PMTs but reconstructible from the MRD track:

$$E_{\mu,\text{outer}} = \beta \cdot d_{\text{outer}} \tag{6.4}$$

where β ≈ 1.992 MeV/cm and d_outer = ||r_tank,exit − r_PMT,exit|| is the distance from the tank boundary to the PMT-volume boundary, extrapolated from the MRD track.

**MRD energy loss** — E_loss,MRD is determined from the muon range and stopping position in the MRD.

The **total reconstructed muon energy** is:

$$E_{\mu,\text{reco}} = \alpha \cdot Q + \beta \cdot d_{\text{outer}} + E_{\text{loss,MRD}} + c \tag{6.6}$$

A linear fit of E_tank vs. Q from simulation yields the conversion factor and offset:

$$\alpha = (0.08534 \pm 0.00169) \text{ MeV/p.e.}$$
$$c = (87.3 \pm 3.9) \text{ MeV}$$

This is consistent with the independent determination α' = (0.0833 ± 0.08) MeV/p.e. from Pershing (2021) using a different data sample. The energy resolution from simulation is μ_energy ≈ 7 MeV with σ_energy ≈ 117 MeV; a high-energy tail arises from events with pion production, where additional Cherenkov light inflates the reconstructed energy.

### Vertex reconstruction

The interaction vertex is extrapolated backward along the MRD track direction d̂_MRD into the water volume, scaled by the charge:

$$\vec{r}_{\text{vertex}} = \vec{r}_{\text{PMT,exit}} - \frac{\alpha}{\beta} \cdot Q \cdot \hat{d}_{\text{MRD}} \tag{6.8}$$

Reconstruction accuracy from simulation: σ_xi ≈ 35 cm for all three spatial coordinates x, y, z. The largest residual is for Δz, where a tail at Δz ≈ −1 m reflects events reconstructed too far upstream.

### Fiducial Volume (FV) definition

Events are restricted to the upstream region of the tank to minimize sky-shine neutron contamination and maximize acceptance for beam-forward neutrons:

$$\rho = \sqrt{x^2 + z^2} < 1.0 \text{ m}$$
$$|y| < 0.5 \text{ m}$$
$$z < 0.0 \text{ m}$$

Muon energy and angular distributions in data agree well with Monte Carlo simulation (Figs. 6.19–6.20), with the dominant population at cos(θ_μ) ≈ 1 (beam-forward muons) and an energy peak around 800–1000 MeV.

---

## Delayed Event Analysis: Neutron Counting

### Expected neutron yield (§6.4.1)

Neutron production is simulated using GENIE (v2.12.0) for the neutrino-nucleus interaction and Geant4/WCSim for particle propagation and secondary interactions. Two neutron categories are distinguished:

- **Primary neutrons**: produced directly in the neutrino-nucleus interaction (e.g., from nuclear breakup or FSIs within the struck oxygen nucleus)
- **Secondary neutrons**: produced in subsequent hadronic rescattering in the water target

GENIE-predicted probability of observing ≥1 primary neutron by interaction channel:

| Channel | P(n ≥ 1) | Comment |
|---|---|---|
| CC-QE | ~34% | All from FSI within nucleus; QE kinematics conserve a proton |
| CC-RES | ~52% | Resonance production more likely to break up nucleus |
| CC-DIS | ~68% | High inelasticity drives highest neutron rates |

Including secondary interactions from Geant4 extends the total neutron multiplicity distribution to ≥30 in rare cases (vs. maximum ~8 for primary only), and significantly increases the fraction of events with at least one neutron relative to considering primary neutrons alone.

### Neutron candidate selection cuts (§6.4.2)

Gadolinium neutron capture signals are faint: **10–40 p.e. across 10–20 PMTs** (vs. ~130 PMT hits for the prompt muon). This requires discrimination from afterpulsing and dark noise clusters.

Selection criteria derived from the AmBe neutron calibration campaign:

$$q_{\text{total}} < 120 \text{ p.e.}$$
$$q_{\text{CB}} < 0.4$$
$$q_{\text{CB}} < 0.5 - \frac{q_{\text{total}}}{300 \text{ p.e.}}$$

where q_CB is the Charge Balance parameter (see [simulation-framework.md](../software/simulation-framework.md) for definition). These cuts define a region in the q_CB vs. q_total plane (lower-left region; Fig. 6.23) that is enriched in neutron-like clusters.

The **neutron search window** is restricted to **10–70 μs** after the beam trigger, excluding the 6–10 μs region where afterpulsing is enhanced (Fig. 6.24 shows the structure of a typical extended-window event, with the prompt muon at ~1000 ns, afterpulsing activity between 6–10 μs, and 4 identified neutron candidates marked with red arrows between 10 and 70 μs).

### Properties of selected neutron candidates (§6.4.3)

**Time spectrum**: The cluster time distribution for selected neutron candidates in beam data is well-described by an exponential plus constant background:

$$f(t) = c + A \cdot e^{-t/\tau} \tag{6.13}$$

Fit results (1769 candidates from beam data):

$$c = (14 \pm 12) \text{ counts/2400 ns}$$
$$\tau = (29 \pm 7) \text{ μs}$$

This is consistent with the theoretical Gd-capture decay time τ_theo = 30 μs, validating the neutron selection.

**Charge spectrum**: Peaks between 10–30 p.e., decreasing slope to ~70 p.e., in good agreement between data and MC.

---

## Integrated Neutron Multiplicity Results (§6.4.4)

Neutron candidates are counted per neutrino candidate event and compared with Monte Carlo predictions. Results are presented for two samples:

### Stopping MRD track sample (all neutrino candidates)

The measured multiplicity distribution is **incompatible with the MC prediction at >5σ**:

$$\chi^2/\text{n.d.f.} = 254.8/9, \quad p < 1 \times 10^{-7}$$

Average neutron yields:

$$\bar{n}_{\text{MC}} = 0.515 \pm 0.007_{\text{stat}}$$
$$\bar{n}_{\text{data}} = 0.272 \pm 0.010_{\text{stat}}$$

Events with n_neutron ≥ 2 are suppressed by more than a factor of 2 in data relative to simulation.

### Fiducial Volume sample (FV-origin muons)

$$\chi^2/\text{n.d.f.} = 20.5/9, \quad p = 0.015 \quad (2.33\sigma)$$

Average neutron yields:

$$\bar{n}_{\text{FV,MC}} = 0.627 \pm 0.031_{\text{stat}}$$
$$\bar{n}_{\text{FV,data}} = 0.287 \pm 0.044_{\text{stat}}$$

No events with 3 or 4 neutrons were observed in the FV data sample due to limited statistics. The less significant disagreement compared with the MRD-only sample is expected since systematic uncertainties (not yet included) will reduce the tension, and the FV sample is also smaller.

The general pattern — models overpredict neutron yield at all multiplicities ≥2 — suggests that hadronic interaction models in Geant4 overestimate secondary neutron production, or that the GENIE neutrino generator produces too many neutrons in FSIs, or both.

---

## Neutron Multiplicity vs. Muon Kinematics (§6.5)

### Method

To investigate which region of the muon parameter space drives the neutron deficit, the neutron multiplicity is analyzed as a **2D function** of reconstructed muon energy E_μ and interaction angle cos(θ_μ), using a forward-folding approach (detector-level comparison, not unfolded to true neutrino kinematics).

Average multiplicity per energy or angle bin i:

$$\mu_i = \frac{1}{N_{\text{total},i}} \sum_j j \cdot n_{ij}$$
$$\sigma_i = \sqrt{\frac{1}{N_{\text{total},i} - 1} \sum_j n_{ij}(j - \mu_i)^2}$$

where n_ij = frequency of observing j neutrons in bin i.

### Energy dependence

Energy range covered: 400–1200 MeV (stopping MRD track) or 600–1200 MeV (FV). **No significant energy dependence is found** in the data for either sample. Average measured multiplicities are approximately 0.25 neutrons/event across all energy bins. The simulation also shows a relatively flat behavior (~0.5 neutrons/event), consistently higher than data. The FV sample shows one outlier bin (900–1000 MeV) with ~0.5, attributed to limited statistics.

### Angular dependence

Angular range: cos(θ_μ) ∈ [0.7, 1.0] (stopping MRD track) or [0.8, 1.0] (FV). **No significant angular dependence is found.** The deficit of ~0.25 neutrons vs. ~0.5 predicted is approximately uniform across all angular bins.

### Conclusion from kinematics

The neutron deficit is **not localized** to any particular region of muon energy or angle space. The overestimation appears to be a global property of the models, not confined to a specific interaction topology. This result is consistent with a systematic overestimation of neutron production in the hadronic models (GENIE FSIs and Geant4 secondary interactions), and **does not point to any particular kinematic regime** that would require special attention in future analyses.

---

## Systematic Uncertainties Outlook (§6.6)

Systematic uncertainties are not yet included in the published results; they are under active investigation by the collaboration. The following sources have been identified.

### Muon reconstruction uncertainty

- Energy resolution: σ_E ≈ 120 MeV
- Angular resolution: σ_θ ≈ 0.17 rad

These create migration between true and reconstructed energy/angle bins, described by migration matrices (Fig. 6.34). Since the neutron multiplicity is approximately flat in both variables, the impact of x-axis bin migration is expected to be small.

### Fiducial Volume selection

Vertex resolution (~35 cm) allows events to migrate across the FV boundary. Since n̄ for the FV sample is currently compatible with the full-tank sample, this systematic is estimated to be negligible for the present analysis. It will become important as statistics increase.

### FMV inefficiency — dirt muon contamination

The FMV efficiency is ε_FMV ≈ 90%, so ~10% of dirt (non-neutrino) events enter the neutrino candidate sample. Dirt events have their own neutron multiplicity n̄_dirt. Using flagged veto events:

$$\bar{n}_{\text{dirt}} = 0.113 \pm 0.010 \quad (N_{\text{dirt,det}} = 1715)$$

For the stopping MRD track sample with n̄_ν,det = 0.272 ± 0.011 and N_ν,det = 3564, the FMV-corrected true neutrino neutron yield is:

$$\bar{n}_\nu = 0.281 \pm 0.012 \tag{6.16}$$

Systematic uncertainty from FMV inefficiency: **σ_sys,FMV = 0.009**. For the FV sample, this systematic is negligible because the vertex constraint already strongly suppresses dirt contamination.

### Neutron detection efficiency

The neutron detection efficiency is mapped as a function of position in the tank using the AmBe calibration source at 5 ports × 5 heights (25 positions; Fig. 6.35). Comparing data and MC efficiency maps:

- Ports 1 and 5: MC features ~10% higher efficiency than data
- Ports 2, 3, 4: good data/MC agreement
- **Average relative difference: ~9%** across all positions

Applying a 9% correction to the MC average neutron yield:

$$\bar{n}_{\text{MC}}(\text{nominal}) = 0.515 \pm 0.007_{\text{stat}}$$
$$\bar{n}_{\text{MC}}(\text{reduced}) = 0.469 \pm 0.007_{\text{stat}}$$

This corresponds to **σ_sys,eff ≈ 0.046 (∼9% relative)**. Since the MC consistently overestimates efficiency, the correction will introduce asymmetric systematic error bars that slightly reduce the significance of the tension between data and simulation.

### Future improvements noted in Nieslony 2022

- Ring-counting classification to isolate CC-0π (pure CC-QE) sample, more directly probing FSIs
- Position-dependent efficiency correction factors for the full 3D tank volume
- Larger event samples from extended run periods (2022+)
- Improved vertex and energy reconstruction techniques (machine learning)

---

## Summary of Key Results

| Observable | Value |
|---|---|
| Neutrino candidates (final selection) | 10,932 |
| Neutron candidates (beam, search window 10–70 μs) | 1,769 |
| Neutron capture time τ (data) | (29 ± 7) μs |
| Average n̄ — MC (stopping MRD track) | 0.515 ± 0.007 |
| Average n̄ — data (stopping MRD track) | 0.272 ± 0.010 |
| Average n̄ — MC (FV) | 0.627 ± 0.031 |
| Average n̄ — data (FV) | 0.287 ± 0.044 |
| Data/MC incompatibility (stopping MRD track) | >5σ (χ²/n.d.f. = 254.8/9) |
| Data/MC incompatibility (FV) | 2.33σ (χ²/n.d.f. = 20.5/9) |
| FMV-corrected n̄_ν | 0.281 ± 0.012 |
| Neutron detection efficiency systematic | ~9% (σ_sys,eff ≈ 0.046) |
| Energy/angle kinematic dependence | Not observed |
