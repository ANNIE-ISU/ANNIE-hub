# Photon Detection System

**Domain:** Detector  
**Links to:** [Detector Overview](overview.md) · [Water Cherenkov System](water-cherenkov-detector.md) · [DAQ System](../software/daq-system.md)  
**Last updated:** 2026-10-28

---
sources:
  - author: "Nieslony"
    year: 2022
    type: dissertation
    institution: "Univ. Mainz"
    notes: "Chapter 3 primary source for PMT and LAPPD specs"
  - author: "He"
    year: 2024
    type: dissertation
    institution: "UC Davis"
    advisor: "Svoboda"
    notes: "Table 3.2 gives model-level PMT inventory; supersedes Nieslony on total count discrepancy"
---

## What This Is

ANNIE uses a hybrid photon detection system combining conventional PMTs and Large-Area Picosecond PhotoDetectors (LAPPDs). The conventional PMTs provide full-volume coverage; LAPPDs provide sub-centimetre spatial resolution and sub-100 ps timing on specific detector faces.

---

## Conventional PMTs

### Inventory (Phase II — He 2024 Table 3.2)

| Batch name | Model | Diameter | Count |
|------------|-------|----------|-------|
| ANNIE | Hamamatsu R5912-100 | 8″ | 44 |
| WATCHBOY | Hamamatsu R7081 | 10″ | 48 |
| WATCHMAN | Hamamatsu R7081-100 | 10″ | 4 |
| LUX | Hamamatsu R7081 | 10″ | 20 |
| ETEL | ETEL D784UKFLB | 11″ | 20 |
| **Table total** | | | **136** |

> **Count discrepancy:** He Table 3.2 sums to 136 PMTs, but the dissertation text (§3.2.6) states 132 PMTs are installed for Phase II. The difference of 4 is unresolved in the source; the table total is preserved here for traceability. Use 132 as the canonical installed count until clarified.

*Note: Several small PMTs on the outside of the Inner Structure were implemented in the WCSim simulation for cosmic-veto purposes but are not part of the constructed ANNIE detector.*

### High Voltage

PMTs are biased at approximately 2 kV from CAEN Model 1734P HV modules housed in a CAEN SY527 mainframe. A dedicated pick-off box, designed at Iowa State University, routes HV to individual PMT channels.

### Electronics / Readout

PMT signals are digitised by 500 MHz Flash ADCs (FADCs) in VME crates. The beam trigger opens a 2 μs acquisition window coinciding with the BNB spill; an extended 70 μs window captures delayed neutron captures on Gd (neutron thermalisation time ~20–30 μs in 0.2% Gd-water).

---

## LAPPDs

### Technology Summary

LAPPDs are 20 cm × 20 cm × <1 cm planar microchannel-plate photodetectors manufactured by INCOM, Inc. Key performance specifications:

| Parameter | Value |
|-----------|-------|
| Active area | 20 cm × 20 cm |
| Thickness | < 1 cm |
| Single-photon timing resolution | < 100 ps (transit time spread) |
| Position resolution | < 1 cm |
| Gain | > 10⁷ |

### Deployment in ANNIE

- **Planned complement:** 5 LAPPDs for Phase II physics running
- **First deployment:** March 2022 (initial commissioning)
- **Placement:** Inserted vertically through "mailbox" slots in the tank lid into designated columns on the Inner Structure. Each column accommodates up to 2 LAPPDs stacked vertically; 8 columns are available, for a maximum of 24 LAPPD positions total.
- **Orientation:** Facing inward into the water volume

### Current Operational Status

*[Update with current LAPPD count and operational status as of 2026.]*

---

## Inner Structure

The Inner Structure is an octagonal steel frame inside the cylindrical tank that mounts the PMTs and provides the LAPPD insertion columns. It is wrapped in Teflon for optical reflectivity and optically isolated from the outer tank wall by a black plastic liner. The geometry of the Inner Structure is implemented in the WCSim simulation from original CAD drawings.

---

## Geometry Reference

In WCSim, the coordinate origin is centred on the FMV; the water tank centre is at coordinates (0, −14.45, 168.1) cm relative to this origin.

*See also: [DAQ System](../software/daq-system.md) for signal readout details.*
