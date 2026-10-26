# Active Projects

**Domain:** Projects  
**Links to:** [Timeline & Milestones](timeline.md) · [Completed Projects](completed.md) · [Decision Log](decisions/) · [Roles](../people/roles.md)  
**Last updated:** 2026-10-26

---

*All threads feed into a unified tank-based reconstruction that expands ANNIE's physics reach beyond MRD-dependent methods. See [Timeline](timeline.md) for how they sequence across the grant period.*

---

## Thread 1: LitePath (ANNIEraytracer) — GPU Ray Tracer

**Lead:** PI (Wetstein) + grad student  
**Status:** 🟢 Active — tuning & validation phase  
**Goal:** GPU-accelerated ray-tracing framework for fast, fully-realistic simulation of Cherenkov light in the ANNIE tank. Models structural shadowing, photodetector geometry, and electronics response. Serves as the forward simulator for ML-based track reconstruction.  
**Current focus:** Tuning detector model against data. Geometry adjusted to match 3D metrological scan of real detector. Integration with LAPyPD complete.  
**Depends on:** LAPPD raw data cleaning (Thread 3)  
**Enables:** SBI/ML reconstruction (Thread 6), CC0pi analysis (Thread 7), beam-timing (Thread 4)  
**Repo:** *[add — will be its own Claude Project]*

---

## Thread 2: LAPyPD — LAPPD Simulator

**Lead:** Grad student  
**Status:** 🟢 Active — improving crosstalk model  
**Goal:** Python-based simulation of LAPPD voltage response (56 channels × 256 samples), including anode crosstalk and photoelectron response. Integrated with LitePath; runnable standalone.  
**Current focus:** Increasing realism of crosstalk behavior to support ML pattern-matching from track hypotheses.  
**Depends on:** Understanding of real LAPPD behavior (Thread 3)  
**Enables:** SBI/ML reconstruction (Thread 6)  
**Repo:** *[add]*

---

## Thread 3: LAPPD Raw Data Cleaning

**Lead:** PI (Wetstein) + grad student + external collaborators  
**Status:** 🟡 Needs dedicated effort  
**Goal:** Characterize and correct systematic artifacts in raw LAPPD data: pedestal miscalibrations, channel offsets, PSEC chip ring-buffer wraparound discontinuity, RF noise uncorrelated with physics events.  
**Current focus:** Characterizing severity and regularity of each artifact type.  
**Depends on:** Nothing — foundational  
**Enables:** All data-driven work (Threads 1, 4, 5, 6, 7, 8)  
**Repo:** *[add]*

---

## Thread 4: Beam-Timing / Bunch Reconstruction

**Lead:** PI (Wetstein) — beam simulation & TOF corrections; PI (Weinstein) — event building & selection  
**Status:** 🟢 Active  
**Goal:** Use ANNIE's sub-nanosecond LAPPD timing to resolve BNB temporal substructure and separate neutrino energy components by arrival time relative to beam RF. First experimental demonstration of the "stroboscopic" technique.  
**Current focus:** Beam simulation and TOF corrections (requires reconstructed vertex/trajectory from LitePath); timing-quality event selection.  
**Collaborators:** Former postdoc — extended original beam-timing studies to include magnetic bending effects  
**Depends on:** LitePath validation (Thread 1); light data format (Thread 5)  
**Enables:** Enhanced CC0pi analysis (Thread 7)  
**Timeline:** Extends through all of Year 1 and part of Year 2

---

## Thread 5: Light Data Format & Data Quality

**Lead:** Postdoc 2 + external grad student + external former grad student (now postdoc)  
**Status:** 🟡 In progress — prerequisite for multiple analyses  
**Goal:** Process the full ANNIE dataset into a lightweight, analysis-friendly format. Systematically characterize Data Quality (DQ) issues across the full dataset. ANNIE has >30,000 beam-coincident LAPPD events; ~14,000 physics-quality at 98–99% purity.  
**Current focus:** *[add current status]*  
**Depends on:** LAPPD data cleaning (Thread 3)  
**Enables:** All physics analyses (Threads 4, 6, 7, 8)

---

## Thread 6: ML / Simulation-Based Inference (SBI) Reconstruction

**Lead:** PI (Weinstein), Postdoc 1 (support)  
**Status:** 🟢 Active — early development  
**Goal:** Infer muon track parameters from full detector light pattern using Neural Likelihood Estimation (NLE) or Neural Posterior Estimation (NPE). Understand degeneracies and sensitivity. Build separate LAPPD and PMT embeddings then combine.  
**Current focus:** CNN/GNN embedding design to reduce (56ch × 256 sample) dimensionality; framework design for PMTs and LAPPDs individually.  
**Depends on:** LAPyPD (Thread 2), LitePath (Thread 1), light data format (Thread 5)  
**Enables:** CC0pi analysis (Thread 7), neutron multiplicity (Thread 8)  
**Notes:** Also considering direct forward-simulation fitting using LitePath as an alternative or complement to SBI.

---

## Thread 7: CC0pi Cross-Section Measurement

**Lead:** Grad student (analysis), PI (Wetstein) (supervision); Postdocs 1 & 2 (detector/DQ support)  
**Status:** 🟢 Active — Year 1 validation phase  
**Goal:** Extend ANNIE CC0pi (charged-current, zero-pion) cross-section to lower energies and an optimized fiducial volume using tank-based reconstruction. First incorporation of LAPPD data into a cross-section analysis.  
**Year 1 focus:** Validate new reconstruction against existing CC0pi result; incorporate full 4-year dataset (tilted and untilted PMT periods).  
**Year 2 focus:** Full physics measurement with LAPPD incorporation.  
**Depends on:** LitePath (Thread 1), data cleaning (Thread 3), light data format (Thread 5)  
**Notes:** Existing Rutgers-led CC0pi result presented at Neutrino2026; ISU expanding it.

---

## Thread 8: Neutron Multiplicity Analysis

**Lead:** PI (Weinstein) for ISU contribution; FSU group for analysis framework  
**Status:** 🟢 Active — underway  
**Goal:** Measure neutron multiplicity in CC neutrino interactions in Gd-water as a function of muon kinematics. ISU contribution: integrate tank-based reconstruction to optimize fiducial volume selection.  
**Current focus:** FSU developing neutron detection/calibration; ISU working on fiducial selection using improved track reconstruction.  
**Depends on:** CC0pi event selection framework (Thread 7)  
**Notes:** Gd-loading enhances neutron detection efficiency. Better vertex localization from LitePath is a key enabling ingredient.

---

## Thread 9: Next-Generation LAPPD Modules (Gen-II)

**Lead:** PI (Wetstein), grad student; Fermilab engineer (electronics); FSU (LAPPD purchase)  
**Status:** 🟢 Active — electronics design in progress  
**Goal:** Instrument Gen-II (capacitively coupled) LAPPDs with new modular, waterproof readout electronics. Evaluate strip vs. pixel readout. Target: installation in ANNIE for 2027 run.  
**Current focus:** Fermilab engineer completing readout electronics design incorporating lessons from Gen-I operation.  
**Timeline:** Core electronics design complete during current grant period; ANNIE installation target 2027

---

## Shared Infrastructure

**SuperSANDI / Hybrid C/S Running**  
**Lead:** Postdoc 1  
Nylon vessel installed inside tank, being filled with Gd-loaded WbLS. Enables hybrid Cherenkov/scintillation detection and sensitivity to sub-Cherenkov-threshold particles. Anticipated 12–20 weeks fall 2026, possible additional 12 weeks 2027.
