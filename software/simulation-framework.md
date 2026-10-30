---
title: "ANNIE Simulation Framework"
domain: software
tags: [simulation, wcsim, genie, geant4, monte-carlo, calibration, tuning]
links_to:
  - software/overview.md
  - physics/neutrino-interactions.md
  - detector/water-tank.md
  - detector/muon-range-detector.md
  - detector/forward-muon-veto.md
last_updated: "2026-10-30"
sources:
  - "Nieslony, Michael T. Towards a neutron multiplicity measurement with ANNIE. Univ. Mainz PhD Dissertation, 2022. Ch. 5."
---

# ANNIE Simulation Framework

ANNIE uses a two-stage Monte Carlo chain: GENIE generates neutrino interactions, and WCSim (a Geant4-based Water Cherenkov detector simulation) propagates particles and models the detector response.

---

## Simulation Chain

```
BNB flux files ──► GENIE (gevgen_fnal) ──► WCSim (Geant4/FTFP_BERT_HP) ──► reconstructed MC
                  [ν interaction]         [particle transport + PMT digitisation]
```

1. **Neutrino flux:** BNB flux files from MiniBooNE/Fermilab
2. **Interaction generator:** GENIE `gevgen_fnal` — generates νμ CC/NC events from the BNB flux; final-state particles passed to WCSim
3. **Detector simulation:** WCSim propagates all final-state particles through the ANNIE geometry; Cherenkov photon transport, PMT quantum efficiency, and charge response are all modelled inside WCSim
4. **Reconstruction:** The same algorithms applied to data are applied to MC output

An independent environment, **rat-pac** (V. Fischer), also exists for neutron studies and future WbLS phase planning. WCSim is the primary framework for Phase II beam analysis.

---

## WCSim

WCSim is a Geant4-based Water Cherenkov simulation originally developed for Hyper-Kamiokande and Super-Kamiokande. The ANNIE-specific version is the `annie` branch of the `ANNIEsoft` fork, maintained by Marcus O'Flaherty.

### Physics list

`FTFP_BERT_HP` — the **High Precision (HP)** neutron component provides accurate cross sections and elastic/inelastic scattering for neutrons below 20 MeV kinetic energy, which is essential for modelling Gadolinium neutron capture.

### Coordinate system

Origin at the centre of the FMV; the water-tank geometric centre is at  
**(x, y, z) = (0 cm, −14.45 cm, 168.1 cm)** relative to this origin.

### PMT types

Four conventional PMT types are simulated in ANNIE Phase II (see Table 5.1):

| PMT name | Model | Size | Expose height | Spherical radius | TTS | Max QE |
|----------|-------|------|--------------|-----------------|-----|--------|
| LUX | R7081 | 10″ | 117 mm | 127.4 mm | 3.4 ns | 25.15% |
| ETEL | D784KFLB | 11″ | 118 mm | 141.7 mm | 1.98 ns | 33.96% |
| ANNIE (HM) | R5912HQE | 8″ | 91.6 mm | 102.1 mm | 2.4 ns | 34.50% |
| Watchboy | R7081 | 10″ | 117 mm | 127.4 mm | 3.4 ns | 25.15% |

LAPPDs are implemented as a separate photosensor type; up to 3 positions per mailbox slot across 8 slots gives a maximum of 24 LAPPD positions.

---

## Modifications to the Baseline Simulation

The following improvements were made to the out-of-the-box WCSim geometry during the Nieslony 2022 Phase II analysis:

### PMT positions (laser scan)

PMT positions were updated from a dedicated 3D laser scan of the installed Inner Structure, processed in MeshLab. PMT glass faces were marked with tape before scanning. A two-step coordinate transformation converts scan → STL → WCSim coordinates via two 4 × 4 matrices.

### PMT holder geometry

PMT holders were added to the Geant4 geometry:
- ANNIE PMT holders: white polyethylene, ≈ 33.5 cm × 21 cm × 2 cm, hole radius ≈ 6 cm
- LUX and ETEL holders: black Schedule 80 PVC, ≈ 43 cm × 15.3 cm × 2 cm

The white holders are highly reflective (R_ANNIE-Holder ≈ 0.90 tuned value); the black PVC holders are less so (R_LUX/ETEL-Holder ≈ 0.70).

### Single-PE charge response functions

Each PMT type is assigned its own single-photoelectron (SPE) charge distribution, extracted from an LED calibration campaign (analysis by Teal Pershing 2020). The calibration fit function (Eq. 5.4) includes a pedestal term, an exponential background, a primary SPE Gaussian, and a secondary Gaussian accounting for dynode gain variance. The SPE distributions were converted from Coulomb units to photo-electrons by applying per-PMT gain correction factors (average gain g_avg ≈ 7 × 10⁶).

A single average distribution per PMT type is used in the simulation. SPE distributions feature:
- Mean ≈ 1 p.e., σ ≈ 0.4 – 0.5 p.e.
- ETEL and LUX distributions slightly wider than Watchboy and ANNIE (HM) PMTs

The WCSim default SK PMT response (which includes a pedestal peak) is replaced by these ANNIE-specific distributions without a pedestal, since pedestals represent baseline noise rather than photon-induced signals.

### Inner Structure reflectivity

The Inner Structure was wrapped in white teflon for Gadolinium material compatibility. This is significantly more reflective than bare steel. The teflon reflectivity was treated as a free parameter and tuned to data (see §Tuning below), converging to **R_InnerStr = 1.00**.

---

## Commissioning Data Comparison

Before detailed tuning, an initial comparison of commissioning data to the baseline simulation revealed a charge scale mismatch:
- Data plateau charge: ~4400 p.e.
- Baseline MC plateau charge: ~3200 p.e.
- Conversion factor: **f_conv ≈ 1.375**

Downstream (direct Cherenkov) PMTs agreed well after scaling; upstream PMTs (which collect reflected light) still showed discrepancies, motivating the full tuning campaign.

---

## Tuning of Parameters

### 5.4.1 Overall light yield: Michel electrons

Michel electrons (from stopped muons decaying via μ⁻ → e⁻ νμ ν̄e) provide a well-understood calibration sample extending up to E_max = (m_μ² + m_e²) / (2 m_μ) ≈ 53 MeV.

**Event selection:**
- A cluster (≥5 PMT hits in 50 ns) coincident with an FMV hit and no MRD track tags a stopping muon candidate
- Charge barycenter must be in the downstream region (excludes upward-going muons)
- Subsequent clusters in Δt_Michel ∈ [0.1, 6.0] μs are electron candidates
- Charge Balance cut q_CB < 0.2 removes noise clusters:

  q_CB = √( ΣQ_i² / Q_sum² − 1/N )

  where Q_sum is the total cluster charge, Q_i the charge on PMT i, and N the number of hit PMTs.

**Observed muon decay time:** τ = (2.01 ± 0.05) μs (vs theoretical 2.1 μs in water).

**QE ratio scan:** The Quantum Efficiency tuning factor r_QE ∈ [1.0, 1.6] uniformly scales QE_nominal(λ) for all PMTs. A χ²-based comparison (Pearson's test of homogeneity, ROOT Chi2Test) finds a minimum at **r_QE,min = 1.30** (χ²/n.d.f. ≈ 1), indicating that WCSim underestimates the effective light collection without this upscaling.

### 5.4.2 Material reflectivity: through-going muons

Through-going muons (FMV hit + tank cluster + MRD track, E_μ = 2 GeV in simulation) probe spatial uniformity and reflective surfaces. The ratio f_upstream = Q_upstream / Q_downstream is sensitive to inner-wall reflectivity.

**Tuning results:**
- Inner Structure teflon reflectivity: **R_InnerStr = 1.00** (maximum physical value)
- With R_InnerStr fixed, QE ratio for downstream charge → **r_QE = 1.20** (global)

**PMT-type-specific QE tuning:** PMT-wise charge display (Figure 5.28/5.30) revealed that Watchboy PMTs overestimate charge while ANNIE (HM) PMTs underestimate it. Two separate scaling factors were introduced:
- r_QE,ANNIE ∈ [1.20, 1.60], r_QE,Other ∈ [0.80, 1.20]
- Preferred center: **r_QE,ANNIE = 1.40, r_QE,Other = 1.00**

**Individual PMT-by-PMT tuning:** As a final step, each PMT i is assigned an effective correction factor R_eff(i) = q̄_data(i) / q̄_MC(i) from the through-going muon sample. This configuration was adopted for all subsequent analysis chapters.

### 5.4.3 AmBe neutron calibration

An Americium-Beryllium (AmBe) neutron source provides an independent calibration of the neutron detection response:

α + ⁹Be → ¹²C* + n  
¹²C* → ¹²C + γ(4.44 MeV)

About 60% of neutrons are accompanied by the 4.44 MeV gamma, which is detected by a BGO crystal + dual SiPM assembly. This gamma tags the neutron emission and triggers the DAQ; a 70 μs window is recorded per trigger to capture the neutron thermalization and capture (τ_neutron ≈ 30 μs).

**Source deployment:** 5 calibration ports on the tank lid (ports 1, 2, 3, 5 on the beam axis; port 4 at 75 cm off-axis). The source is lowered to 5 heights per port (z = ±100 cm, ±50 cm, 0 cm), providing a 3D scan of neutron detection efficiency across the detector volume.

**Simulation:** 5 MeV mono-energetic neutrons are used (the tagged-neutron spectrum extends up to ≈7 MeV).

**ANNRI-Gd de-excitation model:** The standard Geant4 Gd capture model is insufficient for a Cherenkov detector. The ANNIE branch of WCSim incorporates the ANNRI-Gd model, based on measurements at the ANNRI germanium spectrometer at J-PARC:
- n + ¹⁵⁵Gd → ¹⁵⁶Gd* → ¹⁵⁶Gd + γ-rays (E_γ = 8.536 MeV total)
- n + ¹⁵⁷Gd → ¹⁵⁸Gd* → ¹⁵⁸Gd + γ-rays (E_γ = 7.937 MeV total)
- Average ~4 gammas per neutron capture; dominant component is a continuum spectrum from quasi-continuum de-excitation; discrete peaks represent (2.78 ± 0.02)% (¹⁵⁶Gd) and (6.94 ± 0.01)% (¹⁵⁸Gd) of captures
- Implemented from public code by the ANNRI-Gd collaboration

**AmBe event selection cuts:**
- Coincident BGO SiPM signal within 200 ns
- No tank cluster in the prompt event window
- Maximum charge < 150 p.e.
- Charge Balance < 0.4 (standard) plus charge-dependent cut: CB(q) < 0.5 − q/300

**Result:** Good charge spectra agreement for ports 1–4 at all heights; port 5 shows slightly wider simulated distribution but acceptable agreement. Validates the ANNRI-Gd model implementation.

---

## Tuning Validation Summary

Four tuning configurations were compared against three independent calibration samples (Table 5.2). χ²/n.d.f. values from Pearson's test of homogeneity:

| Tuning configuration | χ²/n.d.f. (Throughgoing) | χ²/n.d.f. (Michel) | χ²/n.d.f. (AmBe) |
|----------------------|--------------------------|---------------------|-------------------|
| Untuned (R_InnerStr=0.55, r_QE=1.00, no holders) | 17.97 | 47.86 | 23.82 |
| Global Q.E. Ratio (r_QE=1.20, R_InnerStr=1.00) | 1.62 | 1.57 | 2.15 |
| PMT-type Q.E. Ratio (r_ANNIE=1.40, r_Other=1.00) | 1.11 | 3.00 | 3.39 |
| Individual Q.E. Ratio (per-PMT correction) | **0.95** | **1.05** | **2.12** |

The individual Q.E. ratio configuration was selected for all analysis in Chapters 6–9. Mean R_charge distributions (Table 5.3) confirm the individual Q.E. configuration achieves the smallest PMT-wise spread (σ ≈ 0.15–0.26) across all samples.

---

## MRD Simulation Validation (§5.5)

The MRD response was validated using beam neutrino events with a coincident tank cluster and MRD track (no FMV hit), compared to GENIE+WCSim.

- **Track angle distributions** (vertical and horizontal) agree well between data and MC for both stopped and through-going muon categories
- **MRD energy loss** for stopped muons shows a reasonable shape match; data features slightly more events at higher energy losses (~600 MeV), likely due to dirt muon contamination and dark noise hits producing artificially long tracks
- The MRD implementation is deemed acceptable; future improvements will focus on additional veto cuts and dark noise rate tuning

---

## Running Simulation Jobs

*[Fill in: job submission, output locations, CPU requirements, example scripts.]*
