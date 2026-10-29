---
title: "LAPPD (Large Area Picosecond PhotoDetector)"
domain: detector
tags: [lappd, mcp, photon-detection, timing, high-voltage, readout]
links_to:
  - detector/photon-detection-system.md
  - detector/trigger-system.md
  - software/toolanalysis.md
  - physics/neutrino-interactions.md
last_updated: "2026-10-28"
sources:
  - "He, Julie. LAPPD Integration in ANNIE. UC Davis PhD Dissertation, 2024. Ch. 4."
---

# LAPPD (Large Area Picosecond PhotoDetector)

**Domain:** Detector  
**Links to:** [Photon Detection Overview](photon-detection-system.md) · [Trigger System](trigger-system.md) · [ToolAnalysis](../software/toolanalysis.md)  
**Last updated:** 2026-10-28

---

## What This Is

Large-Area Picosecond PhotoDetectors (LAPPDs) are 20 cm × 20 cm microchannel-plate photomultiplier tubes manufactured by **Incom Inc.** (Devens, MA), developed in partnership with DOE, Argonne National Laboratory, and the University of Chicago. ANNIE's Phase II detector contains **five LAPPDs**, making it one of the first large-scale water Cherenkov detectors to run LAPPDs in a physics configuration.

Their transit-time spread (σ ~50 ps) and 2D position readout from 28 anode striplines give them capabilities far beyond conventional PMTs: ANNIE uses them to improve vertex resolution from ~38 cm (PMTs alone) to ~12 cm (PMTs + 5 LAPPDs).

---

## Technology

### Microchannel Plate (MCP) Operating Principle

A conventional PMT accelerates photoelectrons from a photocathode through a dynode chain; gain comes from secondary emission at each dynode. Timing is limited by the spread in electron transit times (~hundreds of ps).

An MCP replaces the dynode chain with a glass plate perforated by millions of microscopic pores (diameter 10–100 μm). A photoelectron enters a pore, strikes the wall, and initiates a secondary-electron cascade that propagates down the pore length. Each single MCP delivers gain ~10³–10⁴. LAPPDs use a **chevron stack**: two MCPs with opposing tilt angles (~8°) so secondary electrons cannot travel straight through without a collision, suppressing ion feedback. The pair gives combined gain ~10⁶–10⁷.

MCP glass is treated by **atomic layer deposition (ALD)** with a resistive layer (to sustain the bias voltage gradient across the plate) and an emissive layer (to maximize secondary electron yield). Incom fabricates the pore structure via the **GCA (Glass Capillary Array) hollow-core process**.

### LAPPD Geometry and Anode Readout

The active stack is only ~2 cm total height. Light enters through the photocathode face; photoelectrons are accelerated across a gap (photocathode bias 2,000–2,500 V) into the lower MCP. The amplified charge cloud exits the upper MCP, crosses an anode gap (100–200 V), and is collected by the stripline anode.

The anode consists of **28 parallel striplines** (each 20 cm long × 5.2 mm wide, 1.7 mm gap between strips). Each strip is read out from both ends:

- **Position along strip:** from the time difference of signals arriving at each end (speed of light along strip)
- **Position across strip:** from charge sharing between adjacent strips (centroid weighting)

This gives full 2D single-photon position reconstruction from one tile.

---

## Key Specifications

| Property | Value | Notes |
|----------|-------|-------|
| Manufacturer | Incom Inc. | Devens, MA |
| Active area | 20 cm × 20 cm | |
| Total stack height | ~2 cm | |
| Anode strips | 28 striplines | 20 cm long, 5.2 mm wide, 1.7 mm gap |
| Photocathode HV | 2,000–2,500 V | |
| MCP HV (per plate) | 900–1,000 V | |
| Gap voltages | 100–200 V each | photocathode→MCP and MCP→anode |
| MCP pair gain | ~10⁶–10⁷ | |
| Transit time spread σ | ~50 ps (design) | |
| Measured timing resolution | 64 ps | LAPPD-63; PILAS 405 nm laser |
| Quantum efficiency | ≥ 25% | recent tiles; LAPPD-63: 26.8 ± 2.55% avg at 420 nm |
| Count in ANNIE Phase II | 5 | |

---

## ANNIE Integration

### System Architecture

Each LAPPD in ANNIE is a complete, self-contained subsystem. The signal and power chain is:

```
LAPPD tile (in watertight housing)
  ↓ 28 striplines → Analog Pickup Board → 2 × ACDC card   [waveform digitisation]
  ↓ LVHV board   [high voltage + low voltage]
  ↓ Trigger card [LAPPD self-trigger]
  ↓ 2 × waterproof cables (~10 m)
  ↓ Breakout Box (BoB)  [RPi 3B; slow controls; power switching; CAN]
  ↓ CAT6 / LVDS
  ↓ ACC (ANNIE Central Card)  [beam gate, PPS, clock; controls ≤8 ACDCs]
  ↓ Ethernet → DAQ PC
```

### Physical Installation: Watertight Housing

The **watertight housing** was designed by Julie He and David Hemer (UC Davis):

| Feature | Detail |
|---------|--------|
| Frame | 13" × 17" × 2" PVC |
| Window | 0.25" UV-transmitting (UVT) acrylic |
| Backplate | Stainless steel |
| Seal | 120 bolts + 2 O-rings (one at acrylic window, one at SS backplate) |
| Feedthroughs | 2 × MacArtney SubConn DBH13F 13-pin socket connectors |
| Interior desiccant | 6–8 silicone gel packets (absorbs residual moisture) |

The housing bolts onto a **deployment panel** (10 ft × 11" × 0.25" PVC). Four polypropylene I-beam sliders bolted to the back of the panel engage the Inner Structure's 8 vertical rails. The bottommost slider acts as a mechanical stopper. Waterproof cables are secured with stainless steel zip ties to prevent them from drifting toward the tank center. Buoyancy is countered by a stainless steel rod at the bottom of the panel.

> **Gd-water compatibility note:** MacArtney SubConn connector rubber is not compatible with Gd-loaded water. Those sections of the cable and socket connectors are wrapped in **PTFE tape**, as is the Inner Structure itself.

> **Soak test validation:** Watertight integrity was confirmed by submerging the fully assembled housing in deionized water for several days (empty, then with electronics installed, first unpowered then powered). No water penetrated the housing. The LVHV humidity sensor and humidity indicator strips monitored conditions; acrylic is slightly permeable to moisture, but the resulting humidity increase was not operationally significant. Frequent opening and closing of the housing requires O-ring replacement.

### Signal Readout: ACDC Card

The **ACDC (Analog Charge to Digital Converter)** waveform digitiser was designed by Eric Oberla at the University of Chicago:

| Parameter | Value |
|-----------|-------|
| ASICs | 5 × PSEC4 |
| Channels | 6 per PSEC4 = 30 total (28 used for strips + 2 spare) |
| Sampling rate | 4–15 GSamples/s (programmable) |
| Analog bandwidth | 1.5 GHz |
| Sample depth | 256 samples per channel (SCA ring buffer ≈ 25 ns window) |
| FPGA | Altera Cyclone-IV GX |
| Revision | Rev B (ANNIE deployment) |
| ACDCs per LAPPD | 2 |

The **ANNIE Analog Pickup Board** (designed by Dr. Bernhard Adams) interfaces the LAPPD's 28 stripline SMA contacts to the two ACDCs using 2 SAMTEC connectors (replacing 60 individual SMA connections), with 4 SMA spares reserved for trigger signals.

### High and Low Voltage: LVHV Board

The **LVHV board** supplies all voltages for the LAPPD and its ACDC readout. Designed by Seth Hillbrand (revised with Paul Stucky):

| Feature | Detail |
|---------|--------|
| Microcontroller | PIC DSPIC33EV256GM102 |
| Communication | CAN bus via MCP2515 |
| Input voltage | +12 V |
| LV outputs | +5 V, +3.3 V (for ACDC and digital logic) |
| HV generation | EMCO C40N module + DAC LTC2631-HZ |
| Sensors | HIH6030-021 (relative humidity + temperature); photodiode (light leak detection) |
| Protection | Under/overvoltage (UV/OV); salt bridge water leak detection |
| Current version | Version 2 |

**Critical operating rule:** HV must be ramped gradually on startup. Sudden voltage changes can permanently damage the LAPPD.

LVHV firmware (written by Julie He) implements automated **HV ramp-down** when temperature or humidity inside the housing exceeds a programmed threshold, protecting the LAPPD from thermal and moisture damage.

### Self-Trigger: Trigger Card

The **trigger card** (designed by Dr. Bernhard Adams) provides a local LAPPD self-trigger independent of the global ANNIE trigger:

| Parameter | Value |
|-----------|-------|
| PCB dimensions | 205 mm × 18 mm |
| Channels | 28 (one comparator per stripline) |
| Threshold DAC | MCP4725; range −1,250 to +1,750 mV |
| Typical threshold | −30 mV |
| Multiplicity logic | Fires when N-of-28 channels exceed threshold |
| Output pulse width | 200 ns |
| Physical connection | Pogo pins contacting the anode striplines |

### Slow Controls and Power Switching: Breakout Box (BoB)

The **BoB** (designed by Seth Hillbrand) services 2 LAPPDs:

- **Raspberry Pi 3B** — master controller
- **Relay HAT** — switches +12 V power to each LVHV board
- **PiCAN 2 HAT** — CAN bus interface to each LVHV board's PIC microcontroller
- **RJ45 ports** — LVDS signals from ACDCs routed over CAT5/6 to the ACC

### Waterproof Cables

Each LAPPD uses 2 **Falmat XtremeNet FM022208-14** cables:

| Property | Value |
|----------|-------|
| Signal lines per cable | 12 |
| Total lines per LAPPD | 24 |
| Length | ~10 m |
| Connector (tank end) | MacArtney SubConn DIL13M (pin) |
| Mating connector (housing) | MacArtney SubConn DBH13F (socket) |
| Gd-water exposure | PTFE-wrapped at connector junction |

### ACC (ANNIE Central Card)

The **ACC** sits outside the tank and coordinates up to 8 ACDCs (= 4 LAPPDs):

| Parameter | Value |
|-----------|-------|
| Designed by | Mircea Bogdan, University of Chicago |
| Firmware | David Greenshields, University of Warwick |
| FPGA | Altera Arria-V |
| Max ACDCs controlled | 8 |
| Signals distributed | Beam gate, PPS (pulse-per-second), master clock |
| Physical link | CAT6 / LVDS |

---

## Characterization

### Quantum Efficiency (QE) Scan

Each tile is QE-mapped on a **5 mm pitch grid** using a 420 nm LED and a NIST-traceable reference photodiode. A complete scan takes approximately 8 hours.

**LAPPD-63 result:** average QE = **26.8 ± 2.55%** at 420 nm  
**Specification for recent Incom tiles:** ≥ 25%

### Timing Resolution

Timing is characterised using a **PILAS PiL040X** pulsed diode laser:

| Parameter | Value |
|-----------|-------|
| Wavelength | 405 nm |
| Pulse width | < 45 ps FWHM |
| Repetition rate | 100 Hz |

Scans run in horizontal, vertical, and high-statistics configurations. **LAPPD-63 measured timing resolution: 64 ps σ**, consistent with the ~50 ps TTS design specification.

### Performance Stability Tests

Long-term stability validation:

- **ACC/ACDC:** High-statistics scans run for ≥ 8 hours/day; any crash or interruption debugged
- **LVHV burn-in:** I²C, SPI, and CAN protocols exercised for several hours; slow controls routine (humidity, temperature, LV output) run to expose software bugs
- **HV burn-in:** HV ramped gradually to nominal using dummy loads (resistance matched to MCPs); LAPPD itself connected after HV tests passed and powered for several hours

---

## Commissioning

Integration was staged through five phases:

1. **Bench characterisation** — QE scan and timing with commercial power supplies
2. **LVHV integration** — LAPPD + ACDC powered from LVHV board; ACC/LVHV still powered externally; laser data confirmed pulses
3. **BoB + cables integration** — +12 V routed through BoB; long cable ground-path drift caused LVDS signal issues, damaging ACC buffer chips → LVHV redesign to mitigate grounding (see Appendix A of He 2024)
4. **Housing integration** — full system enclosed; dark-box test with green LED confirmed LAPPD sees light through acrylic window while housed
5. **Soak test** — fully assembled housing submerged in deionized water; watertightness confirmed

**Deployment mechanism testing** was conducted at the D0 Assembly Building (DAB) at Fermilab before the inner structure was moved to the experimental hall. All 8 panels were cycled on their rail sliders; a custom cross-shaped PVC hook with handle was used to practice retrieval.

**First light in ANNIE tank:** On **March 29, 2022**, the first LAPPD was deployed in the ANNIE detector. The BNB neutrino beam spill was immediately visible in the data — a histogram of trigger rates vs. time showed a clear peak with **FWHM = 1.6 μs**, matching the beam spill duration and confirming the LAPPD detects beam neutrinos.

---

## Operational Status

As of He 2024 (June 2024):

- 1 LAPPD deployed since March 2022
- Beam spill signal confirmed
- LAPPD datastream integration with full ANNIE DAQ in progress
- Phase II goal: 5 LAPPDs total

> See Yue's dissertation (forthcoming) for updated operational status and multi-LAPPD running.

---

## Physics Impact

Simulation studies (He 2024 §4.3, Fig. 4.5) quantify the gain from adding LAPPDs:

| Configuration | Vertex Resolution | Efficiency |
|---------------|-------------------|------------|
| 128 PMTs only | 38 cm | 85% |
| 128 PMTs + 5 LAPPDs | 12 cm | 90% |

The ~3× improvement in vertex resolution is the primary physics motivation for the LAPPD subsystem. Improved vertex reconstruction directly enhances:

- **Neutron multiplicity measurements** — better vertex localisation reduces ambiguity in neutron capture matching
- **Final-state interaction (FSI) studies** — precise interaction vertex required for FSI model discrimination
- **Muon ring reconstruction** — tighter Cherenkov ring edge determination

---

## See Also

- [Photon Detection System](photon-detection-system.md) — full PMT inventory and hybrid photon detection architecture
- [Trigger System](trigger-system.md) — global ANNIE trigger and LAPPD event matching
- [ToolAnalysis](../software/toolanalysis.md) — modular DAQ and reconstruction framework
- [Dissertations Index](../references/dissertations.md) — source dissertations
