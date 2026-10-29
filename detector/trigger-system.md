# Trigger System

**Domain:** Detector  
**Links to:** [Detector Overview](overview.md) · [DAQ System](../software/daq-system.md)  
**Last updated:** 2026-10-28

---
sources:
  - author: "Nieslony"
    year: 2022
    type: dissertation
    institution: "Univ. Mainz"
    notes: "Chapter 3 — TCLK trigger logic and acquisition windows"
---

## What This Is

The trigger system determines which events are recorded. ANNIE uses beam-coincident triggering tied to the BNB spill via Fermilab's Timing Clock (TCLK) system, plus calibration and off-beam triggers.

---

## Beam Trigger

ANNIE receives two TCLK signals from the Fermilab accelerator complex:

| Signal | Name | Meaning |
|--------|------|---------|
| `$1D` | BNB Preparation | Notifies that the BNB is being prepared to fire |
| `$1F` | Booster Extraction Sync | Sent when protons are actually extracted from the Booster |

The **beam trigger** fires when both `$1D` and `$1F` arrive in coincidence:

```
Beam trigger  =  $1D  AND  $1F
```

Upon a beam trigger the DAQ opens a **2 μs acquisition window** for the PMTs, timed to coincide with the full BNB proton spill (~1.6 μs, comprising 81 bunches × 19 ns spacing). An **extended 70 μs window** follows (or runs in parallel) to capture delayed neutron captures on Gd, which occur on a thermalisation timescale of ~20–30 μs.

---

## Off-Beam / Calibration Trigger

When `$1F` arrives **without** a preceding `$1D` (i.e., an extraction without a prepared BNB pulse), ANNIE records the event as an **off-beam** sample. This provides a same-conditions background dataset with no neutrino beam contamination, used for background estimation and calibration.

```
Off-beam / calibration trigger  =  $1F  AND  NOT $1D
```

---

## Front Muon Veto (FMV)

The FMV does not trigger the DAQ independently but is read out in coincidence with beam triggers. An FMV hit in either of its two scintillator layers causes that event to be classified as a potential "dirt muon" candidate (a muon produced upstream of the tank by a neutrino interaction in the surrounding rock). FMV OR logic: a hit in *either* layer is sufficient to flag the event.

See [Detector Overview](overview.md) for subsystem efficiency numbers.

---

## Calibration Sources

*[Fill in: LED flasher trigger, AmBe neutron source trigger, cosmic trigger details.]*

---

## Known Issues / Current Status

*[Fill in: any known trigger inefficiencies or ongoing work.]*
