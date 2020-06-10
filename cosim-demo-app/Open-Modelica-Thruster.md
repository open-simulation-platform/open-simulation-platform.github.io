---
layout: default
title: "Open Modelica Thruster"
parent: "Demo Cases"
nav_order: 2
has_toc: false
permalink: /cosim-demo-app/Open-Modelica-Thruster
---

# Open Modelica Thruster

## System Description

This example describes a simple azimuth thruster model implemented in Open Modelica. 

## Model Description

**Table 1:** *Input and output variables of the quarter truck fmus for connection*

| Name          | I/O   | Unit | Description                                                       |
| ------------- | ----- | ---- | ----------------------------------------------------------- |
| `thrusterOn` | I     | - | Enable or disable thruster|
| `DesiredFractionOfMaxThrust` | I | [0...1] | Desired thrust in percentage of max thrust |
| `alpha` | I     | deg |Desired azimuth |
| `nu_r[1]`    | I     | m/s |Vessel velocity relative to current in surge |
| `nu_r[2]`    | I     | m/s |Vessel velocity relative to current in sway  |
| `nu_r[3]`    | I     | m/s |Vessel velocity relative to current in heave |
| `nu_r[4]`    | I     | rad/s |Vessel velocity relative to current in roll  |
| `nu_r[5]`    | I     | rad/s |Vessel velocity relative to current in pitch |
| `nu_r[6]`    | I     | rad/s |Vessel velocity relative to current in yaw |
| `power_available`    | I | W | Power available to thruster |
| `power`    | O     | W |Power consumed by thruster |
| `tau_actual`    | O     | N | Thrust  |
| `tau_estimated`    | O     | [0...1] |Estimated thrust as a percentage of rated thrust |

## Simulation Results

The following plots show the model response given the setpoints in Table 2. Estimated thrust vs. produced thrust, as percentage of max thrust, is shown in Figure 1, power consumption in Figure 3 and produced thrust in Figure 2.

**Table 2:** *Setpoints for thruster model*

| Name          | Value |
| ------------- | ----- |
| `thrusterOn` | True   |
| `DesiredFractionOfMaxThrust` | 0.5 |
| `alpha` | 0 |
| `nu_r[1]`    | 0 |
| `nu_r[2]`    | 0 |
| `nu_r[3]`    | 0 |
| `nu_r[4]`    | 0 |
| `nu_r[5]`    | 0 |
| `nu_r[6]`    | 0 |
| `power_available` Inf |

[![foo](/assets/img/open-modelica-thruster/thrust_setpoint.png "Desired thrust vs. estimated.")](/assets/img/open-modelica-thruster/thrust_setpoint.png)

**Figure 1:** *Desired vs. estimated thrust as a percentage of max thrust*

[![foo](/assets/img/open-modelica-thruster/thrust.png "Thrust in N")](/assets/img/open-modelica-thruster/thrust.png)

**Figure 2:** *Produced thrust in N*

[![foo](/assets/img/open-modelica-thruster/power.png "Power in W")](/assets/img/open-modelica-thruster/power.png)

**Figure 3:** *Power consumed in W*
