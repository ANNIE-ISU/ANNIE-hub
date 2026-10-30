---
title: "Diffuse Supernova Neutrino Background (DSNB)"
category: physics
tags:
  - dsnb
  - supernova
  - inverse-beta-decay
  - atmospheric-neutrinos
  - water-cherenkov
  - gadolinium
sources:
  - "Nieslony 2022 dissertation, Ch. 7"
  - "Beacom & Strigari 2006 (DSNB flux review)"
  - "Horiuchi et al. 2009 (DSNB flux models)"
related_articles:
  - physics/neutron-multiplicity.md
  - detector/trigger-system.md
analyses: []
---

# Diffuse Supernova Neutrino Background (DSNB)

## What Is the DSNB?

The Diffuse Supernova Neutrino Background (DSNB), sometimes called the supernova relic neutrino (SRN) flux, is the integrated flux of neutrinos and antineutrinos emitted by all core-collapse supernovae (CCSNe) throughout the history of the observable universe.  Each CCSNe emits ~3×10⁵³ erg primarily as ν/ν̄ pairs; integrated over cosmic time and all directions, these produce a nearly isotropic, continuous relic background at Earth.

Detection of the DSNB would provide a unique observational window into the cosmic star formation history, the CCSNe rate at high redshift, the equation of state of hot nuclear matter, and potentially physics beyond the Standard Model (neutrino lifetime, non-standard interactions).

### Flux Spectrum

The ν̄_e DSNB differential flux at Earth is given by integrating the CCSNe emission spectrum over redshift:

$$\phi_{\bar\nu_e}(E_\nu) = \frac{c}{H_0} \int_0^{z_{\rm max}} \frac{R_{\rm SN}(z)\, F_{\bar\nu_e}[(1+z)E_\nu]}{h(z)}\, dz$$

where *R*_SN(*z*) is the volumetric CCSNe rate, *F*_ν̄_e is the time-integrated single-supernova ν̄_e spectrum (Fermi–Dirac with effective temperature ~4–5 MeV), and *h*(*z*) = [Ω_M(1+*z*)³ + Ω_Λ]^½ encodes the cosmological expansion.  The integral redshifts all contributions and dilutes the flux by luminosity distance.

Predicted DSNB flux models span roughly φ_DSNB ~ 1–10 cm⁻²s⁻¹ above 12 MeV; they diverge primarily due to uncertainty in the high-redshift CCSNe rate and the fraction of failed supernovae (collapsar black holes, which emit harder but briefer ν spectra).

### Experimental Status

The Super-Kamiokande (SK) experiment set world-leading upper limits on the ν̄_e DSNB flux:
- φ(E_ν > 17.3 MeV) ≤ (1–30) cm⁻²s⁻¹MeV⁻¹ (90% CL), depending on spectral model assumptions
- SK-Gd (Gadolinium phase, EGADS/SK VII onward) is expected to achieve the first detection within ~5 years of operation

No confirmed detection has been made as of the time of writing (Nieslony 2022).

---

## Detection Channel: Inverse Beta Decay (IBD)

The primary detection channel for ν̄_e DSNB in water Cherenkov detectors is **Inverse Beta Decay (IBD)**:

$$\bar\nu_e + p \;\to\; e^+ + n$$

Key properties:
- **Threshold**: E_ν ≥ 1.806 MeV
- **Cross-section**: σ_IBD ≈ 9.5 × 10⁻⁴² cm² × (E_e/MeV)² (leading order)
- **Kinematics**: The positron carries nearly all the antineutrino energy, *E*_e⁺ ≈ *E*_ν − 1.3 MeV.  The recoil neutron is nearly at rest.
- **Signature**: Prompt Cherenkov light from the positron + its annihilation gammas, followed by a delayed (~20–200 μs) capture signal from the neutron
- **Delayed tag**: In gadolinium-loaded water, neutron capture on Gd (n + Gd → Gd* → Gd + ~8 MeV γ cascade) gives a high-efficiency, low-background delayed coincidence tag

### Signal Energy Window

The relevant IBD analysis window for DSNB searches in water Cherenkov detectors is approximately **12–30 MeV** (positron visible energy):

- **Lower bound ~12 MeV**: Below this, reactor and geo-neutrino ν̄_e backgrounds dominate
- **Upper bound ~30 MeV**: Above this, atmospheric ν̄_e CC interactions (well-measured background) begin to dominate, and the DSNB flux itself falls steeply

Within this window, the expected event rate is O(1–10) events per year per kiloton of target water.

---

## Backgrounds in the DSNB Search Window

Three categories of backgrounds mimic the IBD signal in the 12–30 MeV window.  All three are addressed in Nieslony 2022 Ch. 7 in the context of a Super-K-like Gd-loaded detector.

### 1. Atmospheric NC Quasi-Elastic (NCQE)

The dominant *reducible* background.  Atmospheric neutrinos (any flavor) interact via neutral-current quasi-elastic scattering on oxygen nuclei:

$$\nu + {}^{16}{\rm O} \;\to\; \nu' + {}^{16}{\rm O}^* \quad\text{(nucleon knockout)}$$

The struck nucleon is ejected, leaving an excited residual nucleus (e.g., ¹⁵N or ¹⁵O) that de-excites by emitting one or more gamma rays.  These gammas Compton-scatter electrons that produce diffuse, subthreshold Cherenkov rings — mimicking a positron signal.  A neutron occasionally accompanies the knockout, providing a false delayed tag.

**¹⁶O nuclear shell model** (as used in Nieslony 2022 simulation):
- 1p-shell states: 1p₁/₂ (bound by ~12.1 MeV) and 1p₃/₂ (~18.4 MeV)
- 1s-shell state: 1s₁/₂ (~23 MeV)
- Spectroscopic factors S_α encode the probability for each knockout channel; proton knockout into excited ¹⁵N states dominates the gamma yield

**Key de-excitation lines from proton knockout on ¹⁶O:**
- 6.32 MeV gamma at 87.2% branching ratio (dominant)
- 9.93 MeV gamma at ~6.4% branching ratio

Because individual gammas are difficult to reconstruct in a water Cherenkov detector, these events appear as broad, low-multiplicity Cherenkov light — the key morphological difference from IBD positrons, which produce clean single rings.

### 2. Atmospheric NC Pion Production (NC-nπ)

Neutral-current resonant neutrino interactions can produce pions:

$$\nu + N \;\to\; \nu' + N' + \pi$$

When the pion is absorbed on a nucleus inside the same target volume, it deposits energy through nuclear de-excitation gammas and hadronic secondaries, again mimicking the IBD topology.  In Nieslony 2022, NC-nπ events are included by upscaling the NCQE simulation sample to account for this process's relative cross-section.

### 3. Invisible Muons

Atmospheric ν_μ CC interactions below the Cherenkov threshold (~160 MeV for muons in water) produce **sub-Cherenkov muons** that are invisible to the detector.  These muons decay via μ → e + ν_μ + ν̄_e, producing a Michel electron.  In the 12–30 MeV window, the Michel electron positron-equivalent signal overlaps with the DSNB positron spectrum.

Invisible muon events are essentially irreducible above ~20 MeV in a water Cherenkov detector without directional tagging, since no prompt Cherenkov signal is present to veto the muon's parent interaction.

---

## Simulation Framework (Nieslony 2022)

### Detector Geometry

The Nieslony 2022 Ch. 7 study uses a Super-K-like water Cherenkov geometry simulated in **WCSim** (Geant4 v10.1 with FTFP_BERT_HP physics list):
- Cylindrical tank: radius ~17 m, half-height ~18 m (fiducial ~22.5 kt)
- ~11,150 inner-detector PMTs (20-inch Hamamatsu)
- Gadolinium mass fraction: *f*_Gd = 0.1%
- Trigger threshold: 25 PMTs firing within 200 ns

This geometry is not ANNIE — it is used to study the DSNB signal and atmospheric backgrounds at Super-K scale in order to benchmark the CNN approach before applying it to ANNIE-scale measurements.

### Signal Simulation

DSNB ν̄_e are injected via IBD with vertex positions uniform in the fiducial volume.  The antineutrino spectrum follows the Horiuchi et al. (2009) models for different CCSNe spectral temperatures.  Positron tracks and annihilation gammas are propagated fully in Geant4; the neutron capture on Gd produces the ~8 MeV gamma cascade used as the delayed tag.

### Background Simulation

Atmospheric NCQE and NC-nπ backgrounds are generated with **GENIE v3.0.6** (`gevgen_atmo`) using:
- FLUKA cosmic-ray atmospheric neutrino flux input files
- Relativistic Fermi Gas (RFG) nuclear model for ¹⁶O
- Nuclear shell model spectroscopic factors for proton/neutron knockout
- Final-state interactions (FSI) in GENIE's intranuclear cascade

Output events are propagated through WCSim for the full detector response, including PMT quantum efficiency, dark noise, and trigger simulation.

---

## CNN Event Discrimination

### Motivation

The NCQE background mimics IBD in energy and timing but differs in **event topology**: IBD produces a single, clean Cherenkov ring (positron) whereas NCQE produces diffuse, multi-gamma light patterns.  This morphological difference motivates machine-learning-based discrimination.

### Architecture

Nieslony 2022 Ch. 7 implements a **Convolutional Neural Network (CNN)** operating on rolled-open-cylinder event displays — a 2D unfolding of the detector surface into a rectangular image with two channels: charge (number of photoelectrons) and time (first photon arrival time per PMT) per pixel.

**CNN architecture:**
- 6 double-convolutional packages, each containing two consecutive convolutional layers
- Kernel sizes: (7,7) → (5,5) → (3,3) → (2,2) (decreasing through the network depth)
- 64 convolutional kernels per layer
- 2×2 Max-Pooling after each package
- Batch Normalization after each activation
- 20% Dropout for regularization
- Final fully connected layers → binary classification output (DSNB-like vs. NCQE-like)

The network is trained on WCSim-simulated samples with labels: DSNB positron events (class 1) and NCQE gamma events (class 0).

### Performance

The CNN achieves meaningful signal/background discrimination in the 12–30 MeV window; ROC curve analysis (AUC) quantifies the trade-off between DSNB signal efficiency and NCQE background rejection.  Specific efficiency and rejection figures at operating points are documented in Nieslony 2022 §7.4.

The key result is that CNN-based discrimination can substantially suppress the NCQE background relative to energy-only cuts, improving the effective sensitivity to the DSNB flux.

---

## ANNIE's Role in DSNB Physics

ANNIE is not a DSNB detector — its fiducial mass (~25 tonnes) is far too small for a competitive DSNB flux measurement.  However, ANNIE directly addresses the primary bottleneck limiting DSNB sensitivity in Super-K-Gd and future large detectors: the **uncertainty in the atmospheric NCQE background**.

### The Limiting Background

NCQE is the dominant reducible background in the DSNB search window.  Its rate depends on:
1. The atmospheric neutrino flux (well-constrained by measurements)
2. The NCQE cross-section on ¹⁶O (poorly measured at DSNB-relevant energies ~100–500 MeV)
3. The nuclear de-excitation model, including neutron multiplicity per interaction

Current NCQE cross-section measurements have O(30–50%) uncertainties at these energies.  This directly propagates into an O(30–50%) uncertainty on the NCQE event rate in the signal window, which limits the statistical significance of any DSNB signal claim.

### ANNIE's Measurement

ANNIE's high-precision **neutron detection system** (LAPPDs + Gd-loaded water) measures:

1. **Neutron multiplicity per atmospheric neutrino interaction** — this directly constrains the fraction of NCQE events that produce a neutron (a key handle for separating NCQE from IBD, since IBD always produces exactly one neutron)
2. **NCQE cross-section via event rate normalization** — combined with reconstructed muon energy from the MRD, ANNIE can measure the NCQE rate as a function of neutrino energy

By constraining the NCQE neutron multiplicity distribution and cross-section at ANNIE, these measurements reduce the systematic uncertainty on the NCQE background prediction in large detectors like SK-Gd and Hyper-Kamiokande.

This makes ANNIE a **precision calibration and background measurement** facility for the broader DSNB search program, even though ANNIE itself cannot detect the DSNB signal.

---

## Analyses

| Analysis | File | Status |
|----------|------|--------|
| ANNIE neutron multiplicity study (Nieslony 2022) | [analysis/neutron-multiplicity-nieslony2022.md](../analysis/neutron-multiplicity-nieslony2022.md) | Published (dissertation) |

---

## See Also

- [physics/neutron-multiplicity.md](neutron-multiplicity.md) — Neutron multiplicity from atmospheric neutrino interactions
- [detector/trigger-system.md](../detector/trigger-system.md) — ANNIE trigger and DAQ overview
