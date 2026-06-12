# Integer-N Charge-Pump PLL — SkyWater 130nm PDK

![License](https://img.shields.io/badge/license-MIT-green)
![Technology](https://img.shields.io/badge/technology-SkyWater%20130nm-blue)
![Status](https://img.shields.io/badge/status-Post--Layout%20Verified-brightgreen)
![Tools](https://img.shields.io/badge/tools-Cadence%20Full%20Flow-red)

> - A fully custom Integer-N Charge-Pump Phase-Locked Loop designed and verified from schematic to GDS-II using the complete Cadence toolchain on SkyWater 130nm CMOS process. All six blocks designed from scratch with zero DRC/LVS violations on the complete top-level layout.


## Key Post-Layout Results

| Parameter | Target | Achieved |
|---|---|---|
| Output Frequency | 400 MHz | 400 MHz |
| Lock Time | < 10 µs | 4.85 µs |
| RMS Jitter | < 10 ps | 4.64 ps |
| Phase Noise | < −100 dBc/Hz @ 1 MHz | −105.96 dBc/Hz @ 1 MHz |
| Power Consumption | < 3 mW | 1.26 mW |
| Core Area | 0.1 mm² | 0.0246 mm² |
| DRC Violations | 0 | 0 |
| LVS Violations | 0 | 0 |

## Architecture

The PLL consists of six custom-designed blocks:

- **Phase Frequency Detector (PFD)** — Detects phase and frequency difference between reference and feedback signals
- **Charge Pump** — Current-steering topology converting PFD output to control voltage
- **Loop Filter** — 3rd order passive filter suppressing high-frequency noise on control voltage
- **VCO** — 5-stage current-starved ring oscillator operating at 400 MHz 
- **Divide-by-2 Block** — First stage of feedback division
- **3-bit Feedback Divider** — Completes the integer-N division in the feedback path


## Tools Used

| Tool | Purpose |
|---|---|
| Cadence Virtuoso | Schematic entry and layout |
| Cadence Spectre | Circuit simulation |
| Cadence Quantus PEX | Parasitic extraction |
| Cadence Genus | Logic synthesis |
| Cadence Innovus | Place and route |
| Cadence Xcelium | Digital simulation |
| SkyWater 130nm PDK | Process design kit |


## Repository Structure

```
├── docs/                  # Design decisions, specifications, learnings
├── schematics/            # Block-level schematic screenshots
├── layout/                # Layout screenshots and GDS exports
├── simulations/           # Simulation waveforms by type
├── netlists/              # Not shared (see netlists/README.md)
└── results/               # Summary table and key waveforms
```

## Author

**Madhav Ghulghule**
M.Tech — VLSI and Embedded Systems
Defence Institute of Advanced Technology (DIAT), Pune
2024

[LinkedIn](#) | [GitHub](#)

