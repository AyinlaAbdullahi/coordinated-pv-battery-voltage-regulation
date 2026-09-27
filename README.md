# Coordinated PV-Battery Voltage Regulation on Weak Distribution Feeders

## Overview

This project simulates voltage rise caused by embedded solar PV on a weak distribution
feeder. The feeder topology is adapted from the IEEE 13-bus test case, scaled down to
415V LV and given conductor parameters typical of Nigerian LV networks. The scenario is
directly relevant to Nigeria's 2026 Net Billing Regulations, which now allow embedded
solar generation (50 kWp to 1.5 MWp) to export power straight onto distribution feeders.

The model tests a control strategy that combines inverter reactive power (Volt-VAR
droop) with battery storage to keep voltage within limits, without curtailing solar
generation.

## Current status

The feeder model and Volt-VAR droop control are complete and verified. Battery
coordination is built and working at one of three PV sites, with the other two in
progress. Curtailment (a last-resort fallback) and the full scenario matrix haven't
been started yet.

## Model

File: `weak_feeder_ieee.slx` (MATLAB Simulink).

GitHub can't render `.slx` files directly, so a screenshot of the model will be added
here as the project develops.

Topology:
- LV source (415V, 50Hz), representing the point just after the MV/LV transformer
- 8 wire segments with R/L values based on real Nigerian LV conductor data
- 9 house/load blocks
- 3 PV inverter sites at 30kW each, so single-site and multi-site penetration can be
  compared directly

Control strategy:
1. Volt-VAR droop: inverters adjust reactive power based on measured voltage, trying to
   bring voltage down before touching real power output.
2. Battery coordination: when Volt-VAR alone isn't enough, batteries at each site absorb
   real power to bring voltage down further.
3. Curtailment (planned): if voltage is still out of bounds after the first two stages,
   curtail real power as a last resort.

## Results so far

| Scenario | Voltage (p.u.) | Notes |
|---|---|---|
| Single 30kW inverter | ~1.009 | Everyday case, negligible impact, controller stays off |
| Three 30kW inverters (90kW combined) | ~1.204 | Worst case, well past the 1.05 limit |
| Volt-VAR response, worst case | Saturates at 10,000 | Controller maxes out as expected |
| Battery response, Site 1, worst case | Saturates at 30,000 W | Confirms the battery stage kicks in correctly once Volt-VAR alone can't cope |

## Why build this

Voltage rise from distributed PV hasn't been much of a concern on the Nigerian grid so
far, largely because supply has historically been the constraint, not excess generation.
The 2026 Net Billing Regulations change that calculus on weak LV feeders. This project
takes control techniques that are already well studied elsewhere and applies them to
that specific, under-examined situation.

## Tools

- MATLAB/Simulink (Specialized Power Systems)
- Python/pandapower, used separately to cross-check the feeder topology

## Author

Abdullahi Ayinla Abiodun, Department of Electrical and Electronics Engineering,
University of Ilorin, Nigeria
