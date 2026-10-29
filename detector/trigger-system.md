# Trigger System

**Domain:** Detector  
**Links to:** [Detector Overview](overview.md) · [Veto Systems](veto-systems.md)  
**Last updated:** 2026-10-28

---
sources:
  - author: "Nieslony"
    year: 2022
    type: dissertation
    institution: "Univ. Mainz"
    notes: "Chapter 3 — TCLK trigger logic and acquisition windows"
  - author: "He"
    year: 2024
    type: dissertation
    institution: "UC Davis"
    advisor: "Svoboda"
    notes: "Chapter 3 §3.2.7 — trigger types, words, CTC timing, delayed window, Gd capture time"
---

## Central Trigger Card (CTC)

All ANNIE subsystems are synchronised by the **Central Trigger Card (CTC)**, which distributes trigger signals to the tank PMT DAQ, MRD DAQ, and LAPPD DAQ. Subsystems match local event timestamps to CTC timestamps within defined coincidence windows:

| Subsystem | Coincidence window with CTC |
|-----------|---------------------------|
| Tank PMTs | 300 ns |
| MRD | 2 ms |
| LAPPD | 3 ms |

---

## Trigger Types and Words

| Word | Type | Description |
|------|------|-------------|
| 5 | **Beam** | BNB spill detected — primary physics trigger |
| 31 | **LED Start** | LED pulser sequence started (water transparency) |
| 33 | **LED ADC** | LED pulser ADC readout event |
| 35 | **AmBe** | Americium-beryllium source run (neutron efficiency calibration) |
| 36 | **MRDCR** | MRD cosmic ray (first+last layer coincidence; no beam required) |

---

## Beam Trigger (Word 5)

ANNIE receives two TCLK signals from Fermilab:

| Signal | Name | Meaning |
|--------|------|---------|
| `$1D` | BNB Preparation | BNB being prepared to fire |
| `$1F` | Booster Extraction Sync | Protons extracted from Booster |

```
Beam trigger  =  $1D  AND  $1F
```

On a beam trigger the DAQ opens a **2 μs prompt window** for the PMTs (covers the full BNB spill of ~1.6 μs: 81 bunches × 19 ns). If ≥1 PMT registers ≥5 PE during this window, an **extended 70 μs window** is opened to capture delayed neutron captures on Gd.

### Delayed Window and Neutron Capture

With Gd loading, the neutron thermalisation and capture time constant is **~30 μs** (vs. ~200 μs in pure water). Measured in He 2024 data: τ = 33 ± 2 μs.

- **0–2 μs (prompt):** muon and primary interaction products
- **2–10 μs:** excluded in analysis (muon after-pulsing contamination)
- **10–70 μs (delayed):** neutron candidate search window

Time-cut efficiency for neutron captures: ~62% (38% of captures fall in the excluded 0–10 μs or beyond 70 μs).

---

## Off-Beam Trigger

```
Off-beam  =  $1F  AND  NOT $1D
```

Provides a same-conditions background sample with no neutrino beam; used for background estimation and calibration.

---

## MRD Cosmic Trigger (Word 36: MRDCR)

Coincidence of hits in the **first and last MRD scintillator layers** — identifies through-going cosmic muons. No beam spill required. Used for MRD alignment and efficiency measurements.

---

## LED Trigger (Words 31, 33)

LED flashers in the water tank pulse at calibrated intensity. Runs are interleaved with physics data-taking to monitor water transparency and PMT gain stability. Word 31 marks the start of an LED sequence; Word 33 marks individual readout events.

---

## AmBe Trigger (Word 35)

An ²⁴¹Am-⁹Be radioactive source is deployed inside the water tank via designated ports. AmBe emits ~4.4 MeV γ rays plus neutrons in coincidence. These runs measure **neutron detection efficiency** as a function of position. He 2024 (Ch. 5–6) reports averaged FV efficiency: **65% (data), 76% (MC)** from Ports 1 and 5 (upstream), heights −50 to +50 cm.

---

## Front Muon Veto (FMV) in Trigger Context

The FMV is read out in coincidence with beam triggers. FMV OR logic: a hit in **either** layer flags the event as a potential dirt muon. In physics analysis, any FMV activity causes the event to be vetoed from the muon candidate sample. See [Veto Systems](veto-systems.md).
