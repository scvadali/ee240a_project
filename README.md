# EE240A: Final Project
### Project report on my opamp design for an LCD sub-pixel display driver for EE240A class at UC Berkeley

## Introduction

This report presents the design and implementation of an amplifier tailored for driving a specific
LCD sub-pixel in a smartwatch application. The subsequent sections detail the complete design
flow, beginning at the derivation of high-level design metrics from the given specifications, preliminary
design calculations, and the realization of a functional design. The final design meets
all specified requirements, as summarized in the table below.

| Parameter                       | Target Spec | Design Spec     |
|----------------------------------|-------------|-----------------|
| Settling Time (Tset)            | ≤ 180 ns    | 159.2 ns (-%) |
| Total error                     | ≤ 0.2%      | ≤ 0.1%          |
| Power Consumption               | ≤ 1.5 mW    | 0.31 mW (-%)  |
| CMRR at DC                      | ≥ 70 dB     | 70 dB        |
| PSRR (VDDL) at DC               | ≥ 50 dB     | 51 dB        |
| PSRR (VDDH) at DC               | ≥ 50 dB     | 64 dB        |
| Phase Margin (PM)               | ≥ 45°       | 51.53°          |
| Figure of Merit                 | ≥ 3.70      | 20.08 (+%)   |

## Op-amp Overview
- **2-Stage Op-Amp with Miller Compensation**
- **Input Stage**: Telescopic Cascode for high gain  
- **Output Stage**: Class-AB common source for improved slew rate

![Op-Amp Schematic](images/opamp_schematic.png)

## From Key Specifications to Initial Design Parameters


Settling time (**Tset**) and the error budget (**ϵbud**) provide a starting point for our design. With a refresh rate of 60 Hz and a display resolution of 272 x 340 pixels, Tset is calculated as:

**Tset = 1 / (Refresh rate × Resolution)  = 1 / (60 × (272 × 340))  ≈ 180 ns**

For a given feedback factor (f), the minimum open-loop DC gain (A0) is:

**A0 = 1 / f * ((1 - ϵs) / ϵs)**

Considering constraints on phase margin (PM) and Tset, the unity gain frequency (fu) is:

**fu ≈ -ln(ϵd) / Tset  = -ln(0.002 - ϵs) / Tset**

The plot below illustrates the minimum A0 and fu for the op-amp based on a specific partitioning of the error budget. A design decision was made to prioritize a higher-gain op-amp over a higher fu. This choice leads to an unequal distribution of the error budget, as indicated by the star in the plot.

![Op-Amp Schematic](images/init_des_params.png)

With these design parameters established, topology selection was guided by additional constraints, including slew rate, predefined output and input common-mode voltages, and the voltage drop across the tail current source in the first stage, among others. The 
**gm/ID methodology** was employed to determine the optimal sizing for each transistor in the core amplifier and to identify the corresponding bias points, as outlined in the flowchart above.
