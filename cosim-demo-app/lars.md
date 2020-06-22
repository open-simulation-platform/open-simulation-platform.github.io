---
layout: default
title: "Launch and Recovery System"
parent: "Maritime Reference Models"
nav_order: 1
has_toc: false
permalink: /cosim-demo-app/lars
---

# Launch and Recovery System

## System Description

The launch and recovery system (LARS) consist of two main subsystems, i.e., the A-Frame and the winch, as shown in Figure 1. Figure 2 depicts the modeling sketch of the LARS. The LARS model of interest has two degrees of freedom (DoF) enabled by the tilting cylinder and the winch drum through the sheave connected to the A-Frame beam.

{%
    include figure.html img="/assets/img/lars/gunnerus_rov.png"
    num="1"
    caption="The LARS on Gunnerus vessel in operation launching the ROV (Photo credit. NTNU)"
%}

{%
    include figure.html img="/assets/img/lars/modelling_sketch.png"
    num="2"
    caption="*Modelling sketch of the LARS"
%}

## Subsystems, Submodels and FMUS

The reference system model is divided into  eight sub-models and FMUs as shown in Figure 3

{%
    include figure.html img="/assets/img/lars/model_structure.png"
    num="3"
    caption="*Model structure diagram  of the LARS"
%}

**Table 1:** *List of FMUs in the LARS system.*

| FMU name                | Input | Output | Default Parameters |
| ----------------------- | ----- |------- | ------------------ |
| *WinchActuatorSetpoint*                  | | winchSetpoint | |
| *WinchController*           | `winchSetpoint`, `loadSpeed`| `motorGain` | Proportional gain K = 10.<br/>  Derivative time constant Td = 0.1.<br/>  Derivative gain limitation N = 10.<br/>  Integral time constant Ti = 0.1. <br/> loadDepth_min max/init length 1m/1000m,/200m |
| *WinchActuator*    | `motorGain`, `motorSpeed`  | `motorTorque` | Max motor output torque T = 10kNm  |
| *Winch*          | `motorTorque1`, `motorTorque2`, `motorTorque3` | `motorSpeed`, `loadSpeed`, `winchForce` | Gear ratio r = 141<br/> Gear efficiency e = 0.925<br/>  Drum inertia i = 1600kgm^2<br/>   Drum diameter d = 1.36m<br/>  Drum width B = 1.1m<br/>  Max wire length wireLmax = 1000m<br/>  Load mass m = 500kg<br/>  Wire diameter D= 0.02m<br/>  Wire Young's Modulus E = 85e9<br/>  Wire damping critical coefficient  c= 0.5<br/>  Total friction coefficient r = 1  |
| *AFrameActuatorSetpoint*          | | `aFrameSetpoint` | |
| *AFrameController*                 | `aFrameSetpoint`, `cylinderSpeed`  | `cylinderGain` | Proportional gain K = 2<br/> Derivative time constant Td = 0.01<br/> Derivative gain limitation N = 10<br/> Integral time constant Ti = 0.01<br/> Cylinder min/max/init length 1.2/1.7/1.7m |
| *AFrameActuator*                 | `cylinderGain`, `cylinderSpeed`  | `cylinderForce`  | Max cylinder output force F = 5000kN |
| *AFrame*                 |  `cylinderForce`, `winchForce` | `cylinderSpeed`  | tilting leg length l=2m<br/> cylinder positions a = 0.46m, b = 1.3572m  |

## Default Scenario Setup

The simulated scenario presents the retrieving of the ROV, as shown in Figure 4. Specifically, From 5s to 95s, retrieving the ROV by spooling in the winch drum.  From 95s to 100s, by retracing the tilting cylinder the A-Frame operates from overboard position to the onboard position.

{%
    include figure.html img="/assets/img/lars/default_scenario.png"
    num="4"
    caption="Simulation scenario retrieving the ROV"
%}

## Simulation Results

Given the default setpoint values in `WinchActuatorSetpoint` and `AFrameActuatorSetpoint` , the tilting angle of the A-Frame and the position of the load are shown in Figure 5. Time step 0.001s.

{%
    include figure.html img="/assets/img/lars/simulation_results.png"
    num="5"
    caption="Tilting angle and the load position"
%}
