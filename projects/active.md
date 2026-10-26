# Active Projects

**Domain:** Projects  
**Links to:** [Completed Projects](completed.md) · [Decision Log](decisions/) · [Roles](../people/roles.md)  
**Last updated:** 2026-10-26

---

*All six threads below feed into a common goal: a unified tank-based reconstruction that expands ANNIE's physics reach beyond MRD-dependent methods.*

---

## Thread 1: LitePath (ANNIEraytracer) — GPU Ray Tracer

**Owner:** Wetstein (lead), Brunner-Huber (implementation & validation)  
**Status:** 🟢 Active — in tuning/validation phase  
**Goal:** A GPU-accelerated ray-tracing framework for fast, fully-realistic simulation of Cherenkov light in the ANNIE tank, including structural shadowing, photodetector geometry, and electronics response. Will serve as the forward simulator for ML-based track reconstruction.  
**Current focus:** Tuning the detector model against data; the geometry has been adjusted to match a 3D metrological scan of the real detector. Integration with LAPyPD is complete.  
**Key links:** *[add repo link — to be its own Claude Project]*  
**Notes:** Will be renamed from ANNIEraytracer → LitePath. GPU-powered; leverages AI code frameworks. Sogarwal also contributes to full simulation chain validation.

---

## Thread 2: LAPyPD — LAPPD Simulator

**Owner:** Brunner-Huber (primary), Wetstein  
**Status:** 🟢 Active — improving realism of crosstalk model  
**Goal:** Python-based simulation of LAPPD voltage response, including crosstalk between anode strips. Integrated with LitePath and runnable standalone.  
**Current focus:** Improving accuracy of crosstalk behavior and photoelectron response to support ML-based pattern matching (matching voltage response to track hypotheses).  
**Key links:** *[add repo link]*  
**Notes:** The LAPPD produces 56 channels × 256 samples per channel of voltage-vs-time data. Accurate simulation of this is prerequisite for the SBI/ML reconstruction (Thread 6).

---

## Thread 3: LAPPD Raw Data Cleaning

**Owner:** Wetstein, Brunner-Huber + outside collaborators  
**Status:** 🟡 Needs dedicated effort  
**Goal:** Understand and correct systematic artifacts in raw LAPPD data before physics use: pedestal miscalibrations, channel offsets, PSEC chip ring-buffer wraparound discontinuity, and RF noise uncorrelated with physics events.  
**Current focus:** Characterizing the severity and regularity of each artifact type.  
**Key links:** *[add any relevant notes or code]*  
**Notes:** Outside collaborators are involved. This is a prerequisite for reliable use of in-situ LAPPD data in the reconstruction and ML frameworks.

---

## Thread 4: Beam-Timing / Bunch Reconstruction

**Owner:** Wetstein (beam simulation, TOF corrections, Brunner-Huber support), Weinstein (event building, event selection, timing quality)  
**Status:** 🟢 Active  
**Goal:** Use ANNIE's sub-nanosecond LAPPD timing to resolve the temporal substructure of the BNB, separating neutrino energy components by their arrival time relative to beam RF. First experimental demonstration of the "stroboscopic" technique proposed by Wetstein et al. (2019).  
**Current focus:** Beam simulation and TOF corrections (Wetstein/Brunner-Huber); event-building and timing-quality selection (Weinstein). Currently limited by TOF corrections that require reconstructed vertex and muon trajectory — depends on LitePath progress.  
**Key links:** *[add repo / note for original 2019 proposal]*  
**Collaborators:** Marvin Ascencio Sosa (former postdoc) — reproduced original studies, extended to include magnetic bending effects.  
**Notes:** Extends over all of Year 1 and part of Year 2. Also feeds CC0pi analysis by providing two distinct energy/flux distributions.

---

## Thread 5: Light Data Format & Data Quality

**Owner:** Sogarwal + external grad student + external former-grad-student (now postdoc)  
**Status:** 🟡 In progress — prerequisite for multiple analyses  
**Goal:** Process the full ANNIE dataset into a lightweight, analysis-friendly data format. Establish a systematic understanding of Data Quality (DQ) issues across the dataset.  
**Current focus:** *[add current status]*  
**Key links:** *[add repo / data location]*  
**Notes:** Many analyses (Thread 4, Thread 6, CC0pi) depend on having clean, processed data in an efficient format. ANNIE has >30,000 beam-coincident LAPPD events, ~14,000 physics-quality with 98–99% purity.

---

## Thread 6: ML / Simulation-Based Inference (SBI) Reconstruction

**Owner:** Weinstein (lead), Sweeney (support)  
**Status:** 🟢 Active — early development  
**Goal:** Build a machine-learning framework to infer muon track parameters from the full detector light pattern — (56 channels × 256 samples) LAPPD waveforms plus PMT hit charges and times. Investigate Neural Likelihood Estimation (NLE) and Neural Posterior Estimation (NPE). Understand degeneracies and sensitivity.  
**Current focus:** Developing CNN/GNN embeddings to reduce data dimensionality; framework design for PMTs and LAPPDs individually before combining.  
**Key links:** *[add repo]*  
**Notes:** Complements the LitePath forward simulator — either SBI using high-statistics simulation, or direct forward-simulation fitting of track hypotheses. Also considering incorporating beam simulations from Thread 4 as a prior. Full-tank inference combines LAPPD + PMT to resolve degeneracies.

---

## Thread 7: CC0pi Cross-Section Measurement

**Owner:** Brunner-Huber (analysis lead), Wetstein (supervision), Sweeney & Sogarwal (detector/DQ support)  
**Status:** 🟢 Active — Year 1 validation phase  
**Goal:** Extend the existing ANNIE CC0pi (charged-current, zero-pion) cross-section measurement using the new tank-based reconstruction. Recover lower-energy events, optimize fiducial volume, and incorporate LAPPD data directly into a cross-section analysis for the first time.  
**Current focus (Year 1):** Validating new reconstruction against existing CC0pi result; incorporating full 4-year ANNIE dataset (including tilted and untilted PMT periods).  
**Next (Year 2):** Full physics measurement with LAPPD incorporation and newly acquired beam data.  
**Key links:** *[add existing analysis note / paper]*  
**Notes:** First analysis to use LAPPD data in a cross-section measurement. Builds directly on LitePath (Thread 1) and LAPPD data cleaning (Thread 3).

---

## Thread 8: Neutron Multiplicity Analysis

**Owner:** Weinstein (ISU lead), FSU group (analysis framework)  
**Status:** 🟢 Active — underway  
**Goal:** Measure neutron multiplicity in CC neutrino interactions in Gd-water as a function of muon kinematics. Requires well-contained fiducial sample — enhanced by tank-based reconstruction.  
**Current focus:** FSU developing neutron detection/calibration framework; ISU integrating enhanced track reconstruction to optimize fiducial volume selection.  
**Key links:** *[add]*  
**Notes:** Gd-loading enhances neutron detection efficiency. Better vertex localization from LitePath is a key enabling ingredient.

---

## Thread 9: Next-Generation LAPPD Modules (Gen-II)

**Owner:** Brunner-Huber, Wetstein; Fermilab engineer (electronics); FSU (LAPPD purchase)  
**Status:** 🟢 Active — electronics design in progress  
**Goal:** Instrument Gen-II (capacitively coupled) LAPPDs with new plug-and-play readout electronics. Evaluate strip vs. pixel readout. Target: installation in ANNIE for 2027 run.  
**Current focus:** Fermilab engineer completing readout electronics design; Brunner-Huber performed initial tests at Fermilab.  
**Key links:** *[add]*  
**Notes:** Consolidates 4 years of ANNIE LAPPD experience into a documented, transferable system.

---

## Shared Infrastructure

**SuperSANDI / Hybrid C/S Running**  
**Owner:** Sweeney (installation lead)  
Hybrid Cherenkov-scintillation (C/S) running with SuperSANDI (nylon vessel, Gd-loaded WbLS). Anticipated 12–20 weeks fall 2026 + possible 12 weeks 2027. Provides sensitivity to sub-Cherenkov-threshold particles.

