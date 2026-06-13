# Implementation Notes

---

## PFD — Design and Verification Challenges

**Initial RTL Implementation**
The initial Verilog implementation described PFD functionality without targeting a specific gate-level architecture. Cadence Genus inferred a significantly larger-than-necessary flip-flop and combinational logic structure. Area and power overhead were unacceptable. Discarded.

**Revised RTL — Architecture-Driven Approach**
Rewritten to explicitly reflect the dual-DFF topology: two `dffa_sync` modules with D tied to VDD, a combinational reset (`Q_ref AND Q_fb`), and NOR-based internal reset generation. Synthesis produced a clean, minimal netlist. Post-synthesis simulation in Xcelium confirmed correct directional response across all three operating conditions.

**Post-Layout Correction — Output Path Delay Mismatch**
Post-layout closed-loop simulation revealed a lock failure not visible in open-loop verification. Root cause: propagation delay asymmetry between the UP and DOWN output paths. At PLL startup, this timing skew caused cycle slips that the loop could not correct, preventing lock acquisition.

Fix: `scs130hd_dlygate4sd3_1` delay cell inserted in the reset path to equalise UP/DOWN delays. This cell simultaneously enforces a guaranteed minimum pulse width on both outputs, eliminating the dead zone. After this fix, the PLL achieved lock within 4.85 µs.

**Note:** UP/DOWN path symmetry must be verified explicitly in post-layout simulation before closing the full loop. The failure mode — apparent loop instability — does not point obviously to the PFD as the root cause.

---

## VCO — Post-Layout Shifts and Their Consequences

The VCO was the most constrained block in the design. Key post-layout shifts after Quantus PEX extraction:

| Parameter | Pre-Layout | Post-Layout |
|---|---|---|
| Kvco | 581 MHz/V | 281 MHz/V |
| Vctrl at 400 MHz | 1.291 V | 1.491 V |
| Max. frequency | 540 MHz | 470 MHz |
| Avg. period jitter | 1.8 ps | 2.70 ps |

The Vctrl operating point at 1.491 V sits near the top of the usable 0.3–1.5 V range. This is the principal layout-level limitation — reducing parasitic capacitance on the delay cell routing would restore tuning headroom.

**Note:** Size the VCO with explicit post-layout margin on both Kvco and Vctrl. A pre-layout Vctrl operating point at or above 1.3 V on Sky130 leaves insufficient headroom after extraction.

---

## Charge Pump — Mismatch vs. Kvco Interaction

Post-layout CP mismatch increased from 0.42% to 1.67%, marginally exceeding the 1.6% design target. The expected consequence — degraded phase noise — did not materialise.

The simultaneous post-layout drop in Kvco (581 → 281 MHz/V) reduced overall PLL sensitivity to CP current errors. The two effects partially cancelled, and the closed-loop phase noise of −105.96 dBc/Hz at 1 MHz offset met the target. RMS jitter of 4.64 ps remained within specification.

**Note:** Post-layout block-level degradation does not map linearly to system-level metric degradation. Full closed-loop post-layout simulation is required before drawing conclusions from individual block results.

---

## Loop Filter — DRC-Driven Component Changes

Nominal design values (R = 3.1 kΩ, C1 = 20 pF, C0 = 6 pF) could not be implemented as-designed due to Sky130 DRC constraints:

- `rpoly_hp` minimum segment length forced R from 3.1 kΩ to 5 kΩ - MIM capacitor aspect ratio rules reduced C1 from 20 pF to 19 pF

As-built values (R = 5 kΩ, C1 = 19 pF, C0 = 6 pF) shifted the filter zero and reduced effective capacitance from 26 pF to 25 pF. Phase margin remained within specification.

**Note:** Verify DRC-allowable dimensions for `rpoly_hp` and MIM capacitors before finalising loop filter values. Sky130 geometry constraints make exact implementation of calculated passive values unlikely.

---

## Recommended Design Order

The dependency chain in a CP-PLL makes design order non-trivial. The order that minimises redesign iterations:

```
VCO → Charge Pump → Loop Filter → PFD → Feedback Divider
```

VCO Kvco and tuning range set the loop filter requirements. Loop filter values set the charge pump current target. Fixing the VCO first prevents upstream spec changes from cascading into completed downstream blocks.