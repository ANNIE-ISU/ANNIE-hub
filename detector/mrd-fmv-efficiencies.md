---
title: "MRD and FMV Paddle Efficiencies"
domain: detector
tags: [mrd, fmv, efficiency, calibration, muon, track-reconstruction]
links_to:
  - detector/muon-range-detector.md
  - detector/forward-muon-veto.md
  - detector/calibration.md
  - physics/neutron-multiplicity.md
last_updated: "2026-10-29"
sources:
  - "Nieslony, Michael T. Towards a neutron multiplicity measurement with ANNIE. Univ. Mainz PhD Dissertation, 2022. Ch. 4."
  - "He, Julie. LAPPD Integration in ANNIE. UC Davis PhD Dissertation, 2024. Ch. 5 (summary figures)."
---

# MRD and FMV Paddle Efficiencies

The Muon Range Detector (MRD) and Front Muon Veto (FMV) are the two muon-tagging subdetectors in ANNIE. Their individual paddle efficiencies must be characterized in order to assess systematic effects on the neutron multiplicity and cross-section analyses. Inefficiencies in the FMV allow dirt muon background events to contaminate the neutrino candidate sample; inefficiencies in the MRD degrade muon track reconstruction, increasing energy and angle uncertainties.

The efficiency characterization described here was developed by Nieslony 2022 using Phase II beam data (runs 2285–2873, December 2020 – June 2021). The He 2024 dissertation references these results at the summary level.

---

## MRD Paddle Efficiencies

### Muon Track Fit Procedure

The MRD track fit algorithm was developed by Marcus O'Flaherty. Only TDC (time) information is available for MRD paddles — no charge. The algorithm identifies **MRD clusters**: groups of ≥4 paddle hits within a 30 ns coincidence window. A valid track fit requires at least 2 hits in vertical paddles and 2 hits in horizontal paddles, enabling a 2D fit in each projection that is then merged into a 3D track.

When a track is found, the energy deposited in the MRD is estimated from the incident angle using the empirical lookup function:

$$\text{Energy loss [MeV/cm]} = \exp(a \cdot \theta) + b + \frac{c}{d - \theta}$$

with fit parameters:

| Parameter | Value |
|---|---|
| a | −3.626 ± 24.09 ln(MeV/cm) |
| b | 3.755 ± 15.68 MeV/cm |
| c | 3.592 ± 14.64 MeV/cm |
| d | 1.67 ± 0.65 rad |

Energy loss ranges from ~5 MeV/cm (normal incidence) to ~30 MeV/cm (θ ≈ 1.5 rad). This energy loss is used in combination with Cherenkov light in the tank to reconstruct the total muon energy for cross-section analyses.

### Efficiency Determination Method

Efficiency is measured using **through-going muons**: events with a reconstructed MRD track that hits the first and last MRD layer. Those two border layers are excluded from the efficiency calculation (they define the selection). Efficiencies are therefore reported for layers 2–10.

For each paddle *i*, the efficiency is:

$$\varepsilon_i = \frac{N_{\text{obs},i}}{N_{\text{exp},i}}$$

where N_exp,i is the number of times the reconstructed track geometrically intersects paddle *i*, and N_obs,i is the number of times that paddle actually registered a hit.

A position-resolved efficiency is also computed:

$$\varepsilon_i(\vec{x}) = \frac{N_{\text{obs},i}(\vec{x})}{N_{\text{exp},i}(\vec{x})}$$

useful for identifying border effects and spatial non-uniformity.

### Geometry Correction

Monte Carlo studies (2 GeV muons in WCSim) show that paddles at the edges of horizontal layers L3, L5, L7, and L9 systematically reconstruct at 80–85% even when the true MC efficiency is 95%. The bias arises from track reconstruction projecting through edge paddles while the muon physically misses. Geometry correction factors are calculated per paddle:

$$c_{\text{geo},i} = \frac{\varepsilon_{\text{MRD,MC}}}{\varepsilon_{\text{rec},i}}$$

and applied to data:

$$\varepsilon_{\text{corr},i} = c_{\text{geo},i} \cdot \frac{N_{\text{obs},i}}{N_{\text{exp},i}}$$

### Measured Phase II MRD Efficiencies

**Event selection**: single reconstructed through-going muon track, < 50 MRD paddle hits (noise rejection).

| Efficiency region | 0–10% | 10–30% | 30–70% | 70–80% | 80–90% | 90–100% |
|---|---|---|---|---|---|---|
| Number of paddles | 10 | 2 | 1 | 8 | 35 | 198 |
| Fraction | 3.9% | 0.8% | 0.4% | 3.1% | 13.7% | 77.6% |

*Table 4.1 from Nieslony 2022. Paddles in the 0–10% bin are powered off due to hardware problems.*

**Summary:**
- All-paddle average: **ε_MRD = (90.3 +9.7/−14.9)%**
- Active-channel average: **ε_MRD(active) = (92.1 ± 7.9)%**
- 91.3% of all paddles operate at ≥80% efficiency
- 3 channels fluctuate between 20–70% over time; under investigation for PMT base replacement

**Spatial uniformity**: Position-resolved scan of an exemplary MRD layer shows >90% efficiency across the layer interior with small dips (<10%) only at inter-paddle borders, consistent with reconstruction resolution effects.

**Temporal stability**: Efficiency distributions are stable across four run periods spanning December 2020 – June 2021 (runs 2285–2873). Mean and median values are constant within measurement uncertainties. The 3 unstable channels are visible as outliers in each period.

---

## Front Muon Veto (FMV) Paddle Efficiencies

### Detector Configuration

The FMV consists of **26 scintillator paddles** arranged in **two layers**, inherited from the CDF experiment. The layers are separated by 1–2 cm. Due to aging of the scintillator material, significant attenuation occurs along the paddle length: efficiency is ~90–100% at the PMT-near end and ~50–60% at the far end. To compensate, the two FMV layers are mounted with **PMTs on opposing sides**; the veto decision is formed as an OR between the two layers, so the high-efficiency end of each layer covers the low-efficiency end of the other.

### Efficiency Determination Method

In-situ efficiency is measured using coincident MRD muon tracks: for events with a reconstructed MRD track, the track is **back-propagated** through the water tank to the FMV plane and checked for spatial coincidence with an FMV paddle. The spatial selection window is Δx = [−1.6, 1.6] m, Δy = [−0.4, 0.6] m (from MC study of MRD track / FMV position differences).

Because the two FMV layers have opposing efficiency gradients and the layer-1 efficiency is folded into the layer-2 event selection (a bias), the position-averaged efficiency of layer 2 is estimated using:

$$\bar{\varepsilon}_{L2} \approx \frac{1}{N_{\text{bins}}} \sum_{i=1}^{N_{\text{bins}}} \frac{N_{\text{obs}}(x_i)}{N_{\text{exp}}(x_i)}$$

which averages position-resolved efficiencies across 5 bins per paddle to remove the systematic bias from the layer-1 efficiency entering the layer-2 numerator and denominator differently.

### Historical Measurements

| Campaign | Method | Layer 1 | Layer 2 | Notes |
|---|---|---|---|---|
| Pre-Phase I (lab, ~2016) | PMT coincidence setup | 90–100% near, 50–60% far | Same | Lab cosmic muons |
| Phase I in-situ (runs 798, 813) | MRD back-propagation | 60–80% avg | 60–80% avg | Consistent with lab |
| Phase II in-situ (runs 2421–2873) | MRD back-propagation | 65–90% per paddle | 65–90% per paddle | See below |

### Measured Phase II FMV Efficiencies

All 26 paddle pairs show efficiency values in the range 65–90% for both layers. The position-resolved efficiency curves confirm the opposing-slope behavior: Layer 1 peaks at the negative-x side, Layer 2 at the positive-x side (ε_close ≈ 90%, ε_far ≈ 60%).

**OR-combined miss probability** per paddle pair at position x:

$$P_{\text{miss},i}(x) = (1 - \varepsilon_{L1,i}(x)) \cdot (1 - \varepsilon_{L2,i}(x))$$

Averaged over all 13 paddle pairs and all positions:

- **Average miss probability: P̄_miss = (4.4 ± 1.6)%**
- **Average FMV tagging efficiency: ε̄_FMV = (95.6 ± 1.6)%**

Higher paddles (greater height) show slightly higher miss probability, with P_miss reaching up to ~9% for the highest paddles at the worst position. The FMV specification requires P_miss < 10%; this is met for all paddles and positions.

**Temporal stability**: The FMV efficiency distribution shows stable mean (~75% per layer) and median over runs 2421–2873 (January–June 2021) with no outlier channels (unlike the MRD). The combined OR efficiency can therefore be treated as constant over the Phase II data-taking period.

---

## Impact on Physics Analyses

### FMV Inefficiency → Background Contamination

FMV misses allow dirt muon events (muons entering the tank from outside) to survive the beam-neutrino event selection. These events must be estimated and subtracted as background in the neutron multiplicity and cross-section analyses. The ~4.4% miss rate sets the scale of this systematic. Additional software-based cuts (e.g., identifying tank PMTs lit near the entry point of the dirt muon) can partially recover additional rejection.

### MRD Inefficiency → Track Reconstruction Degradation

Inefficient MRD paddles reduce the number of successfully reconstructed tracks. The impact was evaluated by comparing a nominal MC (95% uniform efficiency) to a realistic MC (per-paddle efficiencies from data):

- **14% reduction** in successfully reconstructed tracks with realistic efficiencies
- Track angle resolution is not strongly biased: σ(Δθ) = 0.173 rad (realistic) vs. 0.174 rad (nominal)
- X-position resolution of track start point is similarly unaffected
- The primary impact is reduced statistics, not biased track quality

With 6 missing paddle hits, track reconstruction efficiency drops to ~60% of nominal. In practice, 0 missing paddles is the most common case (66%), with 1 missing (25%) and 2 missing (7%) accounting for most of the remainder.

The MRD track reconstruction inefficiency enters the neutron multiplicity analysis as an increased statistical uncertainty (fewer events pass the muon selection). It is treated as a systematic effect in the analysis, as detailed in Nieslony 2022 Ch. 6.

---

## See Also

- [Muon Range Detector](../detector/muon-range-detector.md)
- [Forward Muon Veto](../detector/forward-muon-veto.md)
- [Detector Calibration](../detector/calibration.md) — AmBe, PMT gain, timing
- [Neutron Multiplicity](../physics/neutron-multiplicity.md) — uses MRD/FMV efficiencies as systematics
