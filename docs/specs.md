# Specifications

## Design Targets

| Parameter | Specification |
|---|---|
| Technology | SkyWater 130nm CMOS |
| Supply Voltage | 1.8 V |
| Reference Frequency | 25 MHz |
| Output Frequency | 200 MHz |
| VCO Frequency | 400 MHz |
| Division Ratio | 8 (÷2 × ÷8 feedback chain) |
| Lock Time | ≤ 10 µs |
| RMS Period Jitter | ≤ 10 ps |
| Output Duty Cycle | 50% |
| CP Mismatch | ≤ 1.6% |

---

## Pre-Layout vs. Post-Layout Results

### System-Level

| Parameter | Pre-Layout | Post-Layout |
|---|---|---|
| Output Frequency | 200 MHz | 200 MHz |
| Lock Time | 2.195 µs | 4.85 µs |
| RMS Period Jitter | 8.7 ps (peak) | 4.64 ps |
| Output Duty Cycle | 49.5% | 49.3% |
| Phase Noise @ 1 MHz offset | −100 dBc/Hz | −105.96 dBc/Hz |
| Core Area | — | 0.0246 mm² |
| Power Consumption | 1.002 mW | 1.260 mW |
| DRC Violations | — | 0 |
| LVS Violations | — | 0 |

### VCO

| Parameter | Pre-Layout | Post-Layout |
|---|---|---|
| Operating Frequency | 400 MHz | 400 MHz |
| Kvco | 581 MHz/V | 281 MHz/V |
| Vctrl at 400 MHz | 1.291 V | 1.491 V |
| Tuning Range | 200–540 MHz | 200–470 MHz |
| Avg. Period Jitter | 1.80 ps | 2.70 ps |
| Duty Cycle | 45.5% | 44.9% |
| Power | 0.502 mW | 0.707 mW |

### Charge Pump

| Parameter | Pre-Layout | Post-Layout |
|---|---|---|
| Nominal Current (ICP) | 60 µA | 60 µA |
| Current Mismatch | 0.42% | 1.67% |

### Loop Filter (As-Built)

| Component | Designed | As-Built | Reason for Change |
|---|---|---|---|
| R | 3.1 kΩ | 5 kΩ | rpoly_hp DRC minimum segment length |
| C1 | 20 pF | 19 pF | MIM capacitor aspect ratio rule |
| C0 | 6 pF | 6 pF | No change |

### Phase Frequency Detector

| Parameter | Value |
|---|---|
| Architecture | Dual-DFF with asynchronous reset |
| Dead-zone elimination | scs130hd_dlygate4sd3_1 delay cell |
| Logic family | Sky130 HD standard cells |
| Flow | RTL (Xcelium) → Synthesis (Genus) → P&R (Innovus) |

### Feedback Divider

| Parameter | Value |
|---|---|
| Architecture | Synchronous 3-bit binary counter |
| Division ratio | ÷8 |
| Flow | RTL (Xcelium) → Synthesis (Genus) → P&R (Innovus) |

### Divide-by-2

| Parameter | Value |
|---|---|
| Function | VCO output buffering + duty cycle correction |
| Output duty cycle | 50% |