# Detector Overview

**Domain:** Detector  
**Links to:** [Water Cherenkov System](water-cherenkov-detector.md) · [Photon Detection System](photon-detection-system.md) · [Trigger System](trigger-system.md) · [Physics Overview](../physics/overview.md)  
**Last updated:** 2026-10-26

---

## What ANNIE Is

ANNIE (Accelerator Neutrino Neutron Interaction Experiment) is a 26-ton gadolinium-loaded water Cherenkov neutrino detector located at Fermilab on the Booster Neutrino Beamline (BNB), in the SciBooNE hall (Building 109). Its scientific goals are to measure neutron production and CC/NC cross sections in neutrino-nucleus interactions, and to serve as a testbed for advanced detection technologies — particularly Large-Area Picosecond PhotoDetectors (LAPPDs) and novel target media.

## Current Configuration (as of 2026)

- **Water volume:** 26 tons, gadolinium-loaded (Gd-water)
- **Photon detectors:** Three Gen-I LAPPDs (wall, downstream of beam) + conventional PMTs
- **Muon Range Detector (MRD):** Downstream of beam; tracks exiting muons
- **Front Muon Veto (FMV):** Rejects muons from upstream neutrino interactions
- **SuperSANDI:** Nylon vessel inside the tank; being filled with Gd-loaded water-based liquid scintillator (WbLS) for hybrid Cherenkov/scintillation running (fall 2026)
- **Phase:** Transitioning from R&D/operations to physics data-taking and analysis

## Key R&D Milestones Achieved

- **2020:** First observation of neutrino-induced neutrons in Gd-loaded water
- **2023:** First published observations of beam neutrino interactions in water-based liquid scintillator (WbLS)
- **2025:** Major PMT repair campaign (Sweeney); tilted PMT holders replaced, detector geometry restored
- **2026:** First observation of beam neutrino interactions in Gd-loaded water using LAPPDs (ISU-led publication)
- **2026:** SuperSANDI nylon vessel installation (Sweeney); hybrid C/S running anticipated fall 2026

## Detector Geometry Challenge

The ANNIE tank's small form factor — dimensions comparable to the size of its photosensors and structural elements — makes conventional water-Cherenkov reconstruction difficult. Photodetector acceptance and shadowing vary substantially across track topologies. This motivates the ISU group's development of the LitePath GPU ray-tracer, which explicitly models all structural detail.

## Collaborating Institutions

- **Iowa State University** (PI: Wetstein, PI: Weinstein) — lead institution; LAPPD systems, reconstruction, physics analysis
- **Florida State University** — Gen-II LAPPD purchase; neutron multiplicity analysis framework
- **Rutgers University** — CC0pi cross-section measurement (Rutgers-led)
- **Fermilab** — site host; engineer supporting Gen-II LAPPD electronics

*[Add full collaboration list]*

## Key References

- ANNIE Collaboration publications: *[add]*
- First LAPPD observation paper (2026): *[add]*
- WbLS paper (2023): *[add]*
