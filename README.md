# Integer-N Charge-Pump PLL — SkyWater 130nm CMOS

![License](https://img.shields.io/badge/license-MIT-green)
![Technology](https://img.shields.io/badge/technology-SkyWater%20130nm-blue)
![Status](https://img.shields.io/badge/status-Post--Layout%20Verified-brightgreen)
![Flow](https://img.shields.io/badge/flow-Cadence%20Full%20Flow-red)

A complete 25–200 MHz Integer-N Charge-Pump PLL designed and verified through the full Cadence mixed-signal ASIC flow on the SkyWater 130nm CMOS process. 
All six blocks are designed from transistor level, verified individually, and integrated into a top-level GDS-II layout with zero DRC and LVS violations.

Most Sky130 PLL implementations available publicly are pre-layout only, built on ngspice and Magic, and do not report closed-loop post-layout results. This design executes the complete industry flow — schematic entry, custom analog layout, Quantus PEX parasitic extraction, post-layout Spectre simulation, RTL synthesis, place-and-route, and GDS-II sign-off — with verified pre-layout and post-layout results reported for every block.

---

## Post-Layout Results

| Parameter | Target | Pre-Layout | Post-Layout |
|---|---|---|---|
| Output Frequency | 200 MHz | 200 MHz | 200 MHz |
| VCO Frequency | 400 MHz | 400 MHz | 400 MHz |
| Lock Time | ≤ 10 µs | 2.195 µs | 4.85 µs |
| RMS Period Jitter | ≤ 10 ps | 2.19 ps | 4.64 ps |
| Duty Cycle | 50% | 49.5% | 49.3% |
| Phase Noise @ 1 MHz | — | −100 dBc/Hz | −105.96 dBc/Hz |
| Core Area | 0.1 mm² | — | 0.0246 mm² |
| Power Consumption | < 3 mW | 1.002 mW | 1.260 mW |
| DRC Violations | 0 | — | 0 |
| LVS Violations | 0 | — | 0 |

---

## Architecture

The PLL uses a mixed-signal implementation: analog blocks are full-custom
layouts in Cadence Virtuoso; digital blocks follow an RTL-to-GDS flow through
Cadence Genus and Innovus with the Sky130 HD standard-cell library.

| Block | Type | Description |
|---|---|---|
| Phase Frequency Detector (PFD) | Digital | Dual-DFF with dead-zone elimination via Sky130 HD delay cell |
| Charge Pump | Analog | Cascode current-mirror, 60 µA, 1.67% post-layout mismatch |
| Loop Filter | Analog | 2nd-order passive, R = 5 kΩ, C1 = 19 pF, C0 = 6 pF |
| VCO | Analog | 5-stage current-starved ring oscillator, 400 MHz |
| Divide-by-2 | Digital | Duty-cycle correction and VCO output isolation |
| Feedback Divider | Digital | Synchronous 3-bit binary counter, divide-by-8 |

---

## Tools

| Tool | Purpose |
|---|---|
| Cadence Virtuoso | Schematic entry, analog layout, Full chip schematic Integration |
| Cadence Spectre | Pre- and post-layout simulation |
| Cadence Quantus PEX | Parasitic extraction |
| Cadence Xcelium | Digital functional simulation |
| Cadence Genus | Logic synthesis |
| Cadence Innovus | Place-and-route, top-level integration, GDS-II |
| SkyWater 130nm PDK | Process design kit |

---

## Repository Structure

```
├── docs/                  # Specifications, design decisions, implementation notes
├── schematics/            # Block-level schematic screenshots
├── layout/                # Layout screenshots and GDS-II exports
├── simulations/           # Pre- and post-layout waveforms by type
├── netlists/              # Not shared (see netlists/README.md)
└── results/               # Summary table and key waveforms
```

---

## Author

**Madhav Ghulghule**  
M.Tech — VLSI and Embedded Systems  
Defence Institute of Advanced Technology (DIAT), Pune — 2026

[LinkedIn](https://www.linkedin.com/in/madhavghulghule/) | [GitHub](https://github.com/madhav181)