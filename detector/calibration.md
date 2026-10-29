---
title: "Detector Calibration"
domain: detector
tags: [calibration, pmt, timing, water-quality, neutron, ambe, lappd]
links_to:
  - detector/photon-detection-system.md
  - detector/lappd.md
  - detector/muon-range-detector.md
  - detector/forward-muon-veto.md
  - physics/neutron-multiplicity.md
last_updated: "2026-10-28"
sources:
  - "He, Julie. LAPPD Integration in ANNIE. UC Davis PhD Dissertation, 2024. Ch. 5."
---

# Detector Calibration

ANNIE requires calibrated photon detectors, well-understood timing offsets, monitored water optical properties, and a mapped neutron detection efficiency. He 2024 Chapter 5 describes the full calibration suite as deployed for Phase II data taking.

---

## PMT Gain Calibration

### Measurement Method

Each Target Tank PMT is gain-calibrated using single-photoelectron (SPE) pulses. A dedicated dark-box setup at UC Davis used a Tektronix AFG 3022B arbitrary function generator to pulse LEDs at the PMTs (WATCHBOY surplus + ETEL units). Voltage was stepped in 100 V increments until an SPE peak was clearly resolved.

The charge distribution is fit with the **DEAP-3600 algorithm**: a sum of three Gaussians (pedestal, SPE, and 2-PE peaks) plus an exponential background. The SPE peak position gives the gain in units of ADC counts per photoelectron, which is then converted to absolute charge (electrons) using the electronics calibration.

### Results

- **Initial mean gain**: 7 × 10⁶ (pre-Phase II)
- **Updated mean gain**: 9 × 10⁶ (after HV adjustments in Phase II)
- All 132 PMTs were characterized and gains recorded in the detector database

### In Situ Monitoring

Six **468 nm LED modules** are mounted on the inner structure of the Target Tank. They provide relative gain monitoring during data-taking without requiring a dark-box run. Any drift in the SPE peak position signals a gain change requiring HV adjustment.

---

## LAPPD Calibration

LAPPD timing and charge calibration is described separately in the [LAPPD article](lappd.md) (He 2024 Ch. 4). LAPPDs were not used in the physics analysis reported in He 2024 Ch. 6–7, so no LAPPD calibration constants appear in the Ch. 5 pipeline.

---

## Muon Veto Efficiencies

### MRD (Muon Range Detector)

Muon Range Detector efficiency is measured in situ using **through-going muons** — beam muons that traverse the full MRD. The per-paddle efficiency is:

```
εᵢ = N_obs,i / N_exp,i
```

where N_exp,i is the number of muon tracks predicted to pass paddle *i* based on the track trajectory reconstructed from all other paddles, and N_obs,i is how many of those actually registered a hit.

- **Typical efficiency**: ≥ 80% for most paddles
- Occasional dead paddles from scintillator aging or PMT failure are identified and masked in analysis

### FMV (Forward Muon Veto)

FMV efficiency per paddle ranges **60–90%** depending on paddle position, PMT condition, and scintillator light yield. Two PMTs on opposite ends of each scintillator paddle provide geometric compensation: if one is less efficient, the other catches more light from that end.

**Combined FMV efficiency**: ~96% — sufficient to veto forward-going muons that would otherwise fake the neutrino interaction topology.

---

## Timing Calibration

### Light Source

A **PILAS PiL040X 405 nm diode laser** (Edinburgh Instruments) delivers calibration pulses through a single-mode fiber to a **PTFE (teflon) diffuser ball** suspended at the centre of the Target Tank. The diffuser ball scatters light isotropically so that all PMTs and LAPPDs receive near-simultaneous illumination.

| Laser parameter | Value |
|---|---|
| Wavelength | 400–405 nm |
| Pulse width | ~30 ps |
| FWHM | 19 ps |
| Timing jitter | 3 ps |

### Calibration Procedure

The time difference between the laser trigger and the recorded PMT hit time gives the **time-of-flight-corrected offset** for each PMT channel. Repeating this measurement for all 132 PMTs yields a per-channel offset table that is stored in the ToolAnalysis database and applied during event reconstruction.

### Residual Uncertainty

- **PMT offset uncertainty**: 1–1.5 ns per channel (dominated by cable length uncertainty and PMT transit time spread)
- After applying the offset table, the **BNB bunch structure** (81 bunches × 53 MHz ≈ 18.9 ns spacing) becomes visible in the beam timing distribution — confirming that sub-nanosecond inter-PMT timing is achieved

---

## Water Transparency and Quality

### Transparency Monitoring

Six LED modules installed on the inner structure (same as gain monitoring) provide **relative optical transparency monitoring**. The standard measurement directs light from the bottom LED module up to the top ETEL PMTs; the ratio of detected to expected photons tracks transparency changes over time.

- Transparency has been **stable since the start of Phase II data taking**
- Drops in transparency are correlated with filter or resin maintenance events in the water recirculation system

### Water Quality Parameters

The ANNIE water system monitors three parameters continuously:

| Parameter | Why it matters |
|---|---|
| Conductivity | Tracks ion concentration; rising conductivity → increased light absorption |
| Temperature | Affects Gd solubility and PMT dark rate |
| pH | Critical for Gd retention (see below) |

### Gadolinium Chemistry

Gadolinium was dissolved in the ANNIE water to achieve ~0.1% Gd₂(SO₄)₃ by mass. Maintaining the correct chemical environment is essential:

- Gd requires a **slightly acidic pH** to remain in solution
- At basic pH, Gd precipitates as **Gd(OH)₃** → white flakes that coat detector surfaces and reduce neutron capture efficiency
- The pH is monitored continuously; if it rises, small amounts of sulfuric acid are added via the water system

---

## Neutron Calibration

### Overview

Neutron capture in ANNIE is primarily on gadolinium (~90% of captures) and hydrogen (~10%, releasing 2.2 MeV). Measuring the **neutron detection efficiency** as a function of tank position requires a tagged neutron source that can be deployed at known locations inside the detector.

### AmBe Source

ANNIE uses a **100 μCi AmBe (americium-241/beryllium-9) sealed source**. The nuclear reactions are:

```
²⁴¹Am → ²³⁷Np + α
α + ⁹Be → ¹²C* + n
¹²C* → ¹²C + γ (4.44 MeV)
```

The **4.44 MeV prompt gamma ray** is used as the trigger tag, signaling that a neutron has just been emitted into the water.

### Source Housing

| Component | Detail |
|---|---|
| Vessel | 2-inch diameter cylindrical acrylic, closed end + conical cap |
| Sealing | Two O-rings under cap + bolts |
| Wiring grooves | Machined along vessel length for flush routing |
| AmBe source location | Base of vessel |
| Scintillator | 2" × 2" BGO (bismuth germanium oxide) crystal on top of source |
| BGO wrapping | White PTFE tape (maximize light collection via reflection) |
| Light readout | Initially two SiPMs (coincidence required); later replaced by a single 2-inch PMT for better signal-to-noise |
| Optical isolation | Source assembly isolated to prevent BGO photons entering the tank |

**BGO properties**: ~8,500 photons/MeV light yield; emission peak 375–650 nm.

An initial design included a stainless steel plate between the source and crystal to block X-rays from ²³⁷Np, but this was removed due to space constraints inside the acrylic vessel.

### Trigger Logic

When the BGO detects the 4.44 MeV gamma:
- **Two-SiPM era**: coincidence between both SiPMs required (≥10 ADC counts above baseline on 2 consecutive samples ≈ 5.9 mV threshold); this reduced backgrounds by a **factor of 25**
- **PMT era**: single PMT threshold crossing
- On trigger: a **67 μs acquisition window** opens to capture all tank waveforms (including any Cherenkov light from Gd de-excitation following neutron capture)

### Deployment

The source is lowered into the tank via a **custom-built winch** through any of the **five calibration ports in the tank lid**. Data are taken at multiple heights per port to build a 2D efficiency map (height vs. port position).

### Neutron Capture Efficiency Map

Two calibration campaigns have been completed (both during periods of no beam):

- Neutron energies from AmBe: up to ~10 MeV — sufficient to escape the acrylic housing and enter the water
- Efficiency metric: (number of hit clusters found in 67 μs window) / (number of BGO triggers)
- **Measured range: ~55–70%** across the tank volume

Efficiency map (height × port, SiPM-era data):

```
Height (m) | Port 1 | Port 5 | Port 2 | Port 3 | Port 4
---------|--------|--------|--------|--------|--------
  +1.0   |  61%  |  71%  |  56%  |  57%  |  68%
  +0.5   |  71%  |  69%  |  67%  |  61%  |  67%
   0.0   |  54%  |  69%  |  69%  |  61%  |   —
  −0.5   |  58%  |  66%  |  64%  |  61%  |  69%
  −1.0   |  57%  |  65%  |  58%  |  54%  |  67%
```

These position-dependent efficiency values are used in Chapter 6 to correct the **neutron production rate** measured in neutrino interactions.

### Backgrounds

Sources of background neutrons (to be characterized in future studies):
- External neutrons entering through the tank walls
- Cosmic muons clipping the detector (spallation products)
- Radioactive isotopes in detector materials

---

## Calibration Summary

| System | Method | Key Result |
|---|---|---|
| PMT gain | SPE dark-box + in situ LEDs | Mean gain 9 × 10⁶ |
| Timing | PILAS 405 nm laser + diffuser ball | 1–1.5 ns offset uncertainty; bunch structure visible |
| FMV efficiency | Through-going muon tracks | ~96% combined |
| MRD efficiency | Through-going muon tracks | ≥ 80% most paddles |
| Water transparency | LED bottom → top PMTs | Stable; correlated with maintenance |
| Neutron efficiency | AmBe BGO-tagged source | 55–70% position-dependent |

---

## See Also

- [LAPPD](lappd.md) — LAPPD-specific characterization (timing, QE)
- [Photon Detection System](photon-detection-system.md) — PMT and LAPPD system overview
- [Muon Range Detector](muon-range-detector.md) — MRD hardware
- [Forward Muon Veto](forward-muon-veto.md) — FMV hardware
- [Neutron Multiplicity](../physics/neutron-multiplicity.md) — physics analysis using these calibrations
