---
title: "ToolAnalysis / ToolDAQ Framework"
domain: software
links_to:
  - software/daq-system.md
  - software/simulation-framework.md
  - detector/trigger-system.md
last_updated: "2026-10-28"
sources:
  - author: "He"
    year: 2024
    type: dissertation
    institution: "UC Davis"
    advisor: "Svoboda"
    notes: "§3.2.8 describes ToolAnalysis architecture and ANNIE usage"
---

# ToolAnalysis / ToolDAQ Framework

**Domain:** Software  
**Links to:** [DAQ System](daq-system.md) · [Simulation Framework](simulation-framework.md) · [Trigger System](../detector/trigger-system.md)  
**Last updated:** 2026-10-28

---

## What This Is

ToolAnalysis is ANNIE's primary offline data-processing and reconstruction framework. It was developed by Dr. Ben Richards at the University of Warwick as a modular, plugin-based system built on top of the **ToolDAQ** library. The same framework is also used by the Hyper-Kamiokande experiment.

---

## Architecture

### Core Design Pattern

Every analysis step is implemented as a **Tool** — a self-contained C++ class with three lifecycle methods:

| Method | Called | Purpose |
|--------|--------|---------|
| `Initialise` | Once at startup | Parse configuration, allocate resources, open files |
| `Execute` | Once per event | Run the analysis logic for the current event |
| `Finalise` | Once at shutdown | Write histograms, close files, release resources |

Tools are chained together in a user-defined sequence stored in a plain-text **toolchain** configuration file. The framework calls each Tool's `Execute` in order for every event, enabling a clean pipeline without hard-coded inter-tool dependencies.

### BoostStores (Shared Data Bus)

Tools communicate through **BoostStores** — key–value stores based on Boost.Serialization. Three standard stores are provided:

| Store | Scope | Typical Contents |
|-------|-------|-----------------|
| `ANNIEEvent` | Per-event | Raw hits, reconstructed quantities, truth info |
| `RecoEvent` | Per-event | Intermediate and final reconstruction outputs |
| `CStore` | Persistent | Run-level metadata, calibration constants |

A Tool writes a quantity by name into a BoostStore; any downstream Tool can retrieve it by the same name. This decoupling allows individual Tools to be swapped, disabled, or reordered without modifying source code.

---

## ANNIE-Specific Tool Chain

The He 2024 analysis pipeline is implemented as a sequence of ToolAnalysis Tools. Key Tools in the chain include:

| Tool | Function |
|------|----------|
| `ZE3RABaseline` | Baseline subtraction on raw ADC waveforms (ZE3RA algorithm) |
| `HitFinder` | Identifies PMT hits above 7 ADC threshold; integrates charge over 50 Ω |
| `ClusterFinder` | Groups hits into clusters using a 50 ns sliding window |
| `ClusterClassifier` | Applies Cherenkov-ball (CB) classifier; removes noise clusters |
| `TimeClustering` | Splits or merges clusters for beam-window event selection |
| `MRDTrackFinder` | Cellular-automation track reconstruction in the MRD |
| `MuonCandidateFilter` | Applies the 7 muon candidate selection cuts |
| `RingEdgeVertex` | Ring-edge vertex reconstruction (He development; see Physics) |
| `MuonEnergyReco` | Iterative Bethe-Bloch muon energy reconstruction |
| `NeutronCandidateFinder` | Applies delayed-cluster cuts for neutron candidate identification |
| `EfficiencyCorrection` | Applies AmBe-derived neutron detection efficiency correction |

---

## Building and Running

ToolAnalysis is built with CMake and requires ROOT, Boost, and the ToolDAQ library. A typical run invocation:

```bash
./Analyse toolchains/MyChain.conf
```

The toolchain config lists Tool names and paths to their individual `.conf` parameter files. Environment variables (`DATA_PATH`, `OUTPUT_PATH`) point to input data and output directories.

---

## Repository

The upstream ToolAnalysis repository is maintained by the University of Warwick. ANNIE maintains a fork with ANNIE-specific Tools at:

```
https://github.com/ANNIEsoft/ToolAnalysisGroup
```

The Hyper-Kamiokande fork demonstrates the framework's portability beyond ANNIE.

---

## See Also

- [DAQ System](daq-system.md) — describes how raw data enters ToolAnalysis
- [Simulation Framework](simulation-framework.md) — WCSim output is processed through the same ToolAnalysis chain
- [Neutrino Interactions](../physics/neutrino-interactions.md) — He 2024 analysis results produced by this pipeline
