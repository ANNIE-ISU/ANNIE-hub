---
title: "LAPPD Slow Controls"
domain: software
tags: [lappd, slow-controls, lvhv, pic, can-bus, raspberry-pi, python]
links_to:
  - detector/lappd.md
  - software/daq-system.md
last_updated: "2026-10-29"
sources:
  - "He, Julie. LAPPD Integration in ANNIE. UC Davis PhD Dissertation, 2024. Appendix B."
---

# LAPPD Slow Controls

The LAPPD slow controls system manages power sequencing, environmental monitoring, and HV/LV control for the LAPPD system in the ANNIE tank. It forms the control plane that sits alongside the ACDC/ACC high-speed data acquisition chain.

---

## System Architecture

```
Surface operators
      │
      ▼ (CAN bus commands)
Raspberry Pi 3B (in Breakout Box)
      │ PiCAN 2 HAT
      ▼ (CAN bus, MCP2515)
LVHV Board PIC microcontroller
      │
      ├─► LV power to ACDC boards
      ├─► HV ramp to LAPPD photocathode / MCPs
      ├─► Power to trigger card
      └─► Sensors: photodiode, RH&T (humidity/temperature)
```

The surface Raspberry Pi sends CAN bus messages to the **DSPIC33EV256GM102 PIC microcontroller** on the LVHV board. The PIC is the embedded controller that actually switches power rails and reads sensors — it is the authoritative interface to all underwater electronics.

---

## PIC Microcontroller

| Parameter | Value |
|---|---|
| Part | Microchip DSPIC33EV256GM102 |
| Role | Embedded controller on LVHV board |
| Communication | CAN bus via MCP2515 transceiver |
| Functions | LV/HV enable, HV set point, sensor readback |

**Source repositories (Julie He's GitHub):**
- PIC firmware: `https://github.com/jujuhub/LVHVPiC-files`
- Python slow controls interface: `https://github.com/jujuhub/lvhv`

---

## Startup Sequence

The LVHV slow controls routine follows a fixed startup sequence before entering the operations loop. **HV must ramp gradually** — never jump to operating voltage in one step.

### Initialization Phase

1. **Power on**: External power supply enabled → Raspberry Pi activates relay HAT → relay closes → LVHV board powers up → PIC starts
2. **PIC initialization**: Configure pin modes, system clock, oscillator, I²C, CAN, SPI, ADC peripherals
3. **Environmental check**: Read photodiode (light level), relative humidity, and temperature from RH&T sensor
4. **Report to surface**: Relay sensor readings back to the surface Raspberry Pi via CAN
5. **LV enable**: Turn on low-voltage rails (+3.8 V, +3.0 V, +1.8 V) to ACDC boards and trigger card
6. **HV ramp**: Set target HV and enable gradual ramp-up to operating voltage (photocathode 2,000–2,500 V; MCPs 900–1,000 V)
7. **Enter operations loop**

### Operations Loop

Once initialized, the PIC runs continuously in an infinite loop:

1. **Receive CAN message** from surface Raspberry Pi — contains a command or data request
2. **Match message ID** to the appropriate code block (enable LV, set HV, read sensor, etc.)
3. **Execute**: send requested data back (RH&T readings, light level, voltage status) or actuate (enable/disable LV or HV)
4. **Read photodiode** — monitor for light leaks inside the watertight housing
5. **Read RH&T** — monitor humidity and temperature; elevated humidity can indicate housing seal failure

---

## Environmental Monitoring

The LVHV board carries sensors specifically to protect the LAPPD during underwater operation:

| Sensor | Part | Monitors | Action if anomalous |
|---|---|---|---|
| Humidity/temperature | HIH6030-021 (SPI version) | Moisture ingress, thermal conditions | Alert operators; safe shutdown |
| Photodiode | — | Light leaks into housing | Alert operators |
| HV read-back | ADM8828 (U506) + resistor divider (÷500) | Actual HV at LAPPD | Compare to set point; shutdown if diverge |
| LV read-back | Voltage monitors | +3.8 V, +3.0 V, +1.8 V rails | Alert operators |

> **Note on the HV read-back divider**: the correct programming resistors divide HV by 500, so a 2,500 V output appears as 5.0 V at the ADC input. Early v2 boards had incorrect resistors (output was 0.36 V at 2,500 V) — see [LAPPD engineering history](../detector/lappd.md#engineering-history).

---

## Salt Bridge Leak Detection

A salt bridge inside the watertight housing detects water ingress: if the housing leaks, water closes the bridge and the resulting conductivity change alerts the monitoring system. This is a last-resort indicator — the humidity sensor typically fires first.

---

## See Also

- [LAPPD](../detector/lappd.md) — hardware description including LVHV board specs
- [DAQ System](daq-system.md) — ACDC/ACC high-speed data acquisition
- [ToolAnalysis](toolanalysis.md) — offline analysis software
