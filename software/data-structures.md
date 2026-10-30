---
title: "ANNIE Data Structures: Channelkeys and Triggerwords"
category: software
tags:
  - channelkey
  - triggerword
  - daq
  - lappd
  - pmt
  - mrd
  - fmv
  - electronics
sources:
  - "Nieslony 2022 dissertation, Appendix A"
related_articles:
  - detector/trigger-system.md
---

# ANNIE Data Structures: Channelkeys and Triggerwords

This article documents two foundational lookup tables used throughout ANNIE's data acquisition, reconstruction, and analysis software: the **channelkey** system (a unique integer identifier for every electronic readout channel) and the **triggerword** system (an integer code assigned to every hardware trigger type by the Central Trigger Card).

---

## Channelkey System

Every electronic channel in ANNIE — PMTs, LAPPD strips, FMV paddles, MRD channels — is assigned a unique integer **channelkey**.  This single integer is used as the primary identifier in raw data files, calibration databases, and analysis software.  Mapping a channelkey back to a physical detector element requires the lookup table below.

### Subsystem Ranges

| Subsystem | Channelkey Range | Count | Notes |
|-----------|-----------------|-------|-------|
| Forward Muon Veto (FMV) | 0 – 25 | 26 | 1 key per paddle (1 sensor per paddle) |
| Muon Range Detector (MRD) | 26 – 331 | 306 | All MRD scintillator readout channels |
| Water Tank PMTs | 332 – 464 | 133 | 1 key per PMT |
| Water Tank LAPPDs | 465 – 765 | 301 | 60 keys per LAPPD × 5 LAPPDs |

Total instrumented channels: 766 (keys 0–765).

### Water Tank PMT Types

The 133 water-tank PMT channelkeys (332–464) cover five PMT models from different procurement campaigns.  Their spatial distribution around the tank wall reflects installation history:

| PMT Type | Count | Description |
|----------|-------|-------------|
| LUX | 20 | 8-inch PMTs originally from the LUX dark matter experiment |
| ETEL | 20 | 8-inch ETL 9354KB PMTs |
| Hamamatsu/ANNIE | 40 | 8-inch Hamamatsu R5912 (ANNIE primary procurement) |
| Watchboy | 48 | 8-inch PMTs from the WATer CHerenkov Monitor for Antineutrinos (Watchboy) prototype |
| Watchman | 4 | 8-inch PMTs from the WATCHMAN project |

The mix of PMT types introduces heterogeneous quantum efficiency, transit time spread, and dark noise characteristics; calibration must account for per-type (and ideally per-PMT) response differences.

### LAPPD Channelkey Assignment

Each Large Area Picosecond Photodetector (LAPPD) contributes **60 channelkeys**: 56 active strip channels plus overhead channels for the analog front-end board.  Five LAPPDs are instrumented, using channelkeys 465–765.

LAPPD strip channels encode the spatial position along the strip dimension with ~1 mm resolution; the orthogonal (strip-length) dimension is reconstructed from the timing difference between signals arriving at each end of the strip.

---

## Triggerword System

The **Central Trigger Card** (CTC) generates and distributes hardware triggers to all ANNIE subsystems.  Each trigger type is assigned a unique integer **triggerword** (range 1–64+).  The triggerword is stored in the raw data header for every readout window, enabling offline sorting and selection of events by trigger type.

### Full Triggerword Table (Appendix A, Table A.2)

| Triggerword | Trigger Name | Description |
|-------------|-------------|-------------|
| 1 | `bes_holdoff` | Beam extraction signal holdoff — suppresses triggers during extraction transient |
| 2 | `Beam_veto1` | Beam-coincident veto, type 1 |
| 3 | `Beam_veto2` | Beam-coincident veto, type 2 |
| 4 | `Delay_beam_trigger1` | Delayed beam trigger (delayed relative to BES), type 1 |
| 5 | `Delay_beam_trigger2` | Delayed beam trigger, type 2 |
| 6 | `Cal_Trigger` | Calibration source trigger (LED, AmBe, etc.) |
| 7 | `Periodic_trigger` | Fixed-rate periodic trigger for pedestal and dark-rate monitoring |
| 8 | `Window_trigger` | Software-defined time window trigger |
| 27 | `CosmicTrigger_1` | Cosmic muon trigger, channel 1 |
| 28 | `CosmicTrigger_2` | Cosmic muon trigger, channel 2 |
| 29 | `CosmicTrigger_3` | Cosmic muon trigger, channel 3 |
| 30 | `CosmicTrigger_4` | Cosmic muon trigger, channel 4 |
| 31 | `LED_start` | LED pulser start signal for PMT timing calibration |
| 32 | `input_sync` | GPS 1-pps synchronization pulse for absolute time-stamping |
| 33 | `MRD_CR_trigger` | MRD cosmic-ray self-trigger |
| 34 | `BE_status_1` | Beam extraction status flag 1 |
| 35 | `BE_status_2` | Beam extraction status flag 2 |
| 36 | `offbeam_1` | Off-beam window trigger, type 1 |
| 37 | `offbeam_2` | Off-beam window trigger, type 2 |

A total of 47 entries are assigned in the full table (Nieslony 2022, Table A.2); the excerpt above lists the most significant entries.  Gaps in the numeric sequence correspond to reserved or unassigned trigger slots.

### Trigger Categories

**Beam triggers** (bes_holdoff, Beam_veto1/2, Delay_beam_trigger1/2): These control the readout window relative to the BNB beam extraction signal (BES).  The main beam readout window opens ~1 μs before BES and closes ~2 μs after; delayed triggers capture late-arriving interactions or secondary particles.

**Calibration triggers** (Cal_Trigger, LED_start): Fired during dedicated calibration runs.  LED triggers fire the LED pulsers that illuminate tank PMTs for transit-time and gain calibration.  AmBe source runs use the Cal_Trigger word.

**Periodic/monitoring triggers** (Periodic_trigger, Window_trigger): Used for continuous pedestal monitoring, dark-rate measurement, and activity-independent data quality checks.

**Cosmic triggers** (CosmicTrigger_1–4, MRD_CR_trigger): Multiple cosmic coincidence triggers allow both muon veto and muon calibration samples to be recorded simultaneously.

**Synchronization** (input_sync): The 1-pps GPS pulse provides an absolute time anchor for correlating ANNIE readout with BNB spill timing from FNAL.

**Off-beam triggers** (offbeam_1/2): Windows opened outside the BNB spill provide a beam-uncorrelated control sample for measuring beam-independent backgrounds.

---

## Usage in Offline Software

In the ToolAnalysis framework, channelkeys and triggerwords appear throughout the `ANNIEEvent` store:

```
// Access PMT waveforms by channelkey
std::map<unsigned long, std::vector<Waveform<unsigned short>>> RawADCData;

// Check trigger type in event header
uint32_t trigword = ANNIEEvent->Get("TriggerWord", trigword);
```

The `ChannelNumToWCSimNum` and related lookup utilities in ToolAnalysis convert between channelkeys, WCSim tube numbers, and physical (x,y,z) positions.

---

## See Also

- [detector/trigger-system.md](../detector/trigger-system.md) — Hardware trigger architecture and timing
