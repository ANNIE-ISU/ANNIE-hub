# Timeline & Milestones

**Domain:** Projects  
**Links to:** [Active Projects](active.md) · [Roles](../people/roles.md)  
**Last updated:** 2026-10-26

---

*Based on the ISU ANNIE proposal. The program is intentionally front-loaded: ~20–30% ANNIE effort in Years 1–2, decreasing to ~10% and ~5% in Years 3–4 as focus shifts to publications.*

---

## Near-Term (Fall 2026 — Year 1 Start)

| Milestone | Thread | Status |
|---|---|---|
| SuperSANDI installation complete | Shared infrastructure | 🟡 In progress |
| SuperSANDI hybrid C/S beam running (12–20 weeks) | Shared infrastructure | ⏳ Upcoming |
| LitePath detector model tuned on data | Thread 1 | 🟡 In progress |
| LAPPD raw data artifact characterization | Thread 3 | 🟡 In progress |
| Beam-timing event selection framework established | Thread 4 | 🟡 In progress |

---

## Year 1 (2027)

### Reconstruction & Simulation
- [ ] LitePath fully validated against WCSim and data control samples
- [ ] LAPyPD crosstalk model improved and validated
- [ ] LAPPD raw data corrections implemented and documented
- [ ] Gen-II LAPPD readout electronics design complete (Fermilab engineer)

### Physics Analyses
- [ ] CC0pi reconstruction validated against existing MRD-based result (Thread 7)
- [ ] Full 4-year ANNIE dataset incorporated into CC0pi analysis (Thread 7)
- [ ] Beam-timing TOF corrections implemented using reconstructed vertex/trajectory (Thread 4)
- [ ] Light data format established and full dataset processed (Thread 5)
- [ ] SBI embedding architecture defined for PMTs and LAPPDs individually (Thread 6)

### Operations
- [ ] SuperSANDI second beam period (possible 12 weeks)
- [ ] Gen-II LAPPD tests at Fermilab

---

## Year 2

### Reconstruction & Simulation
- [ ] SBI/ML reconstruction validated on control samples (through-going muons)
- [ ] Single-LAPPD inference benchmarked against full-tank inference
- [ ] LitePath fast enough for production-scale track fitting

### Physics Analyses
- [ ] CC0pi full physics measurement with expanded fiducial sample (Thread 7)
- [ ] First LAPPD data incorporated into CC0pi cross-section (Thread 7)
- [ ] Beam-timing / bunch reconstruction analysis reaching maturity (Thread 4)
- [ ] Neutron multiplicity fiducial volume optimization complete (Thread 8)

### Hardware
- [ ] Gen-II LAPPD(s) installed in ANNIE for second SuperSANDI beam period

---

## Years 3–4 (~10% and ~5% effort)

- [ ] CC0pi cross-section paper submitted
- [ ] Beam-timing / bunch reconstruction paper submitted
- [ ] Neutron multiplicity paper (with FSU) submitted
- [ ] LAPPD technology paper / documentation finalized
- [ ] Software preservation and archival (LitePath, LAPyPD, analysis code)
- [ ] Transition of ANNIE knowledge and software for long-term use

---

## Key Dependencies (Critical Path)

```
LAPPD raw data cleaning (Thread 3)
        │
        ├──→ LitePath tuning (Thread 1) ──→ CC0pi full measurement (Thread 7)
        │           │                               │
        │           └──→ Beam-timing TOF (Thread 4) ┘
        │
        └──→ Light data format (Thread 5) ──→ SBI/ML framework (Thread 6)
                                                      │
                                    LAPyPD (Thread 2) ┘
```

*LAPPD data cleaning (Thread 3) and the light data format (Thread 5) are the highest-leverage near-term priorities — most other threads are gated on them.*

---

## Upcoming Deadlines & Events

*[Add conferences, paper deadlines, beam schedules, review dates here.]*

| Date | Event | Relevant threads |
|---|---|---|
| Fall 2026 | SuperSANDI beam running | Shared infrastructure |
| 2027 | Gen-II LAPPD installation target | Thread 9 |
| *[add]* | *[conference]* | *[threads]* |
