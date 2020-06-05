---
layout: default
title: "Gunnerus-DP"
parent: "Demo Cases"
nav_order: 2
has_toc: false
permalink: /cosim-demo-app/Gunnerus-DP
---

# Gunnerus-DP

The purpose of this demo case is to demonstrate separation of control systems and simulators in `libcosim` and the use of network FMUs.

This toplogy is common when performing hardware-in-the-loop (HIL) simulations. In a HIL setup, the control system is deployed on production hardware and hooked up to an external simulator. The external simulator can be deployed to whatever hardware is available as long as it enables an interface compatible with the control system. The same toplogy can also be seen when running software-in-the-loop (SIL) simulations. SIL is typically used to test and verify system functionality and it is not a requirement that the control system is deployed to production hardware.

This demo case demonstrates that doing simulations with this kind of topology is feasible using `libcosim` and the OSP configuration format. The case simulates a DP controlled vessel performing a box maneuver while exposed to external forces. NTNU's research vessel, Gunnerus (Figure 1), has been used as reference for both the vessel and control plant.

{% include figure.html
    img="/assets/img/gunnerus-dp/gunnerus.jpg"
    num="1"
    caption="Gunnerus"
%}

## System Description

The HIL/SIL toplogy is emulated by splitting the simulation into two instances: one running the control system (Figure 2) and another running the simulator models (Figure 3). Each instance can run separately on different hardware. A single system configuration (Figure 4), where the control plant and simulator is executed in the same instance, is also provided.

The control system and simulator communicates using a network FMU, custom made for this demo. Each system instantiates a version with an interface for receiving and sending states. For instance, the network FMU in the control system sends thruster commands and receives vessel and environment states. The single system configuration does not use the network FMU.  

A list of all FMUs are given in Table 1.

{% include figure.html
    img="/assets/img/gunnerus-dp/control_system.png"
    num="2"
    caption="Control System Configuration"
%}

{% include figure.html
    img="/assets/img/gunnerus-dp/simulator.png"
    num="3"
    caption="Simulator Models Configuration"
%}

{% include figure.html
    img="/assets/img/gunnerus-dp/single_system.png"
    num="4"
    caption="Single System Configuration"
%}

**Table 1:** *List of FMUs in the Gunnerus-DP case.*

| FMU name                | Description                                                                                                                             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| *[Box Reference](#box-reference-model)*      | Provides setpoints for a box maneuver                                                                               |
| *[Control System Communication](#control-system-communication)*           | Network FMU handling communication between control system and simulator |
| *[Current Model](#current-model)*    | Current model generating surface current velocities    |
| *[DP Controller](#dp-controller)*          | 3-Dof DP controller |
| *[Reference Model](#reference-model)*                 | Reference model providing setpoints to DP
| *[Simulator Communication](#simulator-communication)* | Network FMU handling communication between control system and simulator. Shown in Figure 2 as FmiUdpAdapter.  |
| *[Thruster Dynamics](#thruster-dynamics)* | Thruster dynamics
| *[Vessel Model](#vessel-model)* | 6-dof vessel model |

Note that all these fmus contain binaries for `win64` only.

## FMU Descriptions

### Box Reference Model

The box reference FMU generates setpoints for north, east and yaw. The model is shown in Figure 5 and the output in Figure 6. The result is a box maneuver which takes the vessel between 0-40m north and 0-40m east. The setpoints are not intended to be directly sent to the DP controller, but should be passed through a reference filter first.

{% include figure.html
    img="/assets/img/gunnerus-dp/box_reference.png"
    num="5"
    caption="Box Reference Model"
%}

{% include figure.html
    img="/assets/img/gunnerus-dp/box_maneuver.png"
    num="6"
    caption="Box Reference Output"
%}

**Table 2:** *Box Reference model inputs and outputs (I/O).*

| Name          | I/O   | Unit | Description                                                      |
| ------------- | ----- | ---- |----------------------------------------------------------------- |
| `Position[1]` | O     | m     | Setpoint North    |
| `Position[2]` | O     | m     | Setpoint East     |
| `Position[3]` | O     | rad   | Setpoint Yaw      |

### Control System Communication

The control system communication FMU is instantiated in the simulator and is responsible for transmitting simulated states to the control system and to receive thruster commands from the DP controller. Its interface is described in Table 3.

**Table 3:** *Control System Communication inputs and outputs (I/O).*

| Name          | I/O   | Unit | Description                                                       |
| ------------- | ----- | ----| ----------------------------------------------------------------- |
| `Measured_Position[1]`    | I     | m | Vessel north position    |
| `Measured_Position[2]`    | I     | m | Vessel east position     |
| `Measured_Position[3]`    | I     | rad | Vessel yaw position      |
| `Measured_Velocity[1]`    | I     | m/s | Vessel surge velocity    |
| `Measured_Velocity[2]`    | I     | m/s | Vessel sway velocity     |
| `Measured_Velocity[3]`    | I     | rad/s | Vessel yaw velocity      |
| `Measured_Wind[1]`        | I     | m/s | Wind velocity in surge    |
| `Measured_Wind[2]`        | I     | m/s | Wind velocity in sway     |
| `Measured_Wind[3]`        | I     | rad/s | Wind velocity in yaw      |
| `Commanded_Thrust[1]`     | O     | N | Desired thrust from DP in surge    |
| `Commanded_Thrust[2]`     | O     | N | Desired thrust from DP in sway     |
| `Commanded_Thrust[3]`     | O     | Nm | Desired thrust from DP in yaw      |

### Current Model

The current model FMU simulates 2D surface current that is used by the vessel model to calculate drag and resistance. It can be enabled or disabled by setting a parameter on the FMU.

The model is based on the Simulink diagram shown in Figure 8 and its interface is described in Table 4 and Table 5. The current velocity vector has six entries, however, only the x and y componentents are non-zero.

{% include figure.html
    img="/assets/img/gunnerus-dp/current_model.png"
    num="7"
    caption="Current Model"
%}

**Table 4:** *Current Model inputs and outputs (I/O).*

| Name          | I/O   | Unit | Description                                                       |
| ------------- | ----- | ---- | ----------------------------------------------------------------- |
| `Current_Velocity[1]`    | O     | m/s |Current velocity in surge    |
| `Current_Velocity[2]`    | O     | m/s |Current velocity in sway    |
| `Current_Velocity[3]`    | O     | m/s |Always zero    |
| `Current_Velocity[4]`    | O     | rad/s |Always zero    |
| `Current_Velocity[5]`    | O     | rad/s |Always zero    |
| `Current_Velocity[6]`    | O     | rad/s |Always zero    |

**Table 5:** *Current Model Parameters.*

| Name          | Default   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `Enable_Current`    | True     | Enables or disables current    |

### DP Controller

This FMU implements Gunnerus' DP control system. The control plant consists of proportional-derivative, integrator and reference feed forward parts, all of which can be independently enabled or disabled.

The controller inputs are the vessel’s position in NED (North-East-Down), velocity in BODY, position setpoint in NED, velocity setpoint in BODY and acceleration setpoint in BODY. The acceleration setpoint is required if the reference feed-forward part of the model is enabled.

A complete list of inputs, outputs and tunables are shown in Table 6 and Table 7.

**Table 6:** *DP Controller inputs and outputs (I/O).*

| Name          | I/O   | Unit |  Description                                                       |
| ------------- | ----- | ---- | ----------------------------------------------------------------- |
| `Vessel_Position[1]`    | I     | m | Vessel north position    |
| `Vessel_Position[2]`    | I     | m | Vessel east position    |
| `Vessel_Position[3]`    | I     | rad | Vessel yaw orientation |
| `Vessel_Velocity[1]`    | I     | m/s | Vessel surge velocity    |
| `Vessel_Velocity[2]`    | I     | m/s | Vessel sway velocity    |
| `Vessel_Velocity[3]`    | I     | rad/s | Vessel yaw velocity    |
| `Position_Setpoint[1]`    | I     | m | Position setpoint north    |
| `Position_Setpoint[2]`    | I     | m | Position setpoint east    |
| `Position_Setpoint[3]`    | I     | rad | Orientation setpoint yaw    |
| `Velocity_Setpoint[1]`    | I     | m/s | Velocity setpoint surge    |
| `Velocity_Setpoint[2]`    | I     | m/s | Velocity setpoint sway    |
| `Velocity_Setpoint[3]`    | I     | rad/s | Velocity setpoint yaw    |
| `Acceleration_Setpoint[1]`    | I     | m/s^2|  Acceleration setpoint surge    |
| `Acceleration_Setpoint[2]`    | I     | m/s^2|  Acceleration setpoint sway    |
| `Acceleration_Setpoint[3]`    | I     | rad/s^2|  Acceleration setpoint yaw    |
| `Commanded_Thrust[1]`    | O     | N | Thrust command in surge    |
| `Commanded_Thrust[2]`    | O     | N | Thrust command in sway    |
| `Commanded_Thrust[3]`    | O     | Nm | Thrust command in yaw    |

**Table 7:** *DP Controller Parameters*

| Name          | Default   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `EnablePDControl`    | True     | Enables or disables proportional derivative control    |
| `EnableIAction`    | True     | Enables or disables integral action    |
| `EnableRefFF`    | True     | Enables or disables reference feed forward    |

### Reference Model

The reference model provides smooth setpoint signals for the DP controller, ensuring a controlled and stable operation. The signals are based on the desired position provided by the box reference FMU.

A simulink representation of the model is shown in Figure 8 and its inputs and outputs are described in Table 8.

{% include figure.html
    img="/assets/img/gunnerus-dp/reference_model.png"
    num="8"
    caption="Reference Model"
%}

**Table 8:** *Reference Model inputs and outputs (I/O).*

| Name          | I/O   | Unit | Description                                                       |
| ------------- | ----- | ---- | ----------------------------------------------------------------- |
| `Position_Setpoint[1]`    | I     | m | User defined setpoint north |
| `Position_Setpoint[2]`    | I     | m | User defined setpoint east |
| `Position_Setpoint[3]`    | I     | rad | User defined setpoint yaw |
| `Desired_Position[1]`     | O     | m | Position setpoint north to DP |
| `Desired_Position[2]`     | O     | m | Position setpoint east to DP |
| `Desired_Position[3]`     | O     | rad | Orientation setpoint yaw to DP |
| `Desired_Velocity[1]`     | O     | m/s | Velocity setpoint surge to DP    |
| `Desired_Velocity[2]`     | O     | m/s | Velocity setpoint sway to DP    |
| `Desired_Velocity[3]`     | O     | rad/s | Velocity setpoint yaw to DP    |
| `Desired_Acceleration[1]` | O     | m/s^2 | Acceleration setpoint surge to DP    |
| `Desired_Acceleration[2]` | O     | m/s^2 | Acceleration setpoint sway to DP    |
| `Desired_Acceleration[3]` | O     | rad/s^2 | Acceleration setpoint yaw to DP    |

### Simulator Communication

The simulator communication FMU is instantiated in the control system and is responsible for transmitting commands to the control system and to receive simulated states vessel and environment models. Its interface is described in Table 9.

**Table 9:** *Simulator Communication inputs and outputs (I/O).*

| Name          | I/O   | Unit | Description                                                       |
| ------------- | ----- | ---- | ----------------------------------------------------------------- |
| `Commanded_Thrust[1]`    | I     | N | User defined setpoint north |
| `Commanded_Thrust[2]`    | I     | N | User defined setpoint east |
| `Commanded_Thrust[3]`    | I     | Nm | User defined setpoint yaw |
| `Measured_Position[1]`     | O     | m | Position setpoint north to DP |
| `Measured_Position[2]`     | O     | m | Position setpoint east to DP |
| `Measured_Position[3]`     | O     | rad | Orientation setpoint yaw to DP |
| `Measured_Velocity[1]`     | O     | m/s | Velocity setpoint surge to DP    |
| `Measured_Velocity[2]`     | O     | m/s | Velocity setpoint sway to DP    |
| `Measured_Velocity[3]`     | O     | rad/s | Velocity setpoint yaw to DP    |
| `Measured_Acceleration[1]` | O     | m/s^2 | Acceleration setpoint surge to DP    |
| `Measured_Acceleration[2]` | O     | m/s^2 | Acceleration setpoint sway to DP    |
| `Measured_Acceleration[3]` | O     | rad/s^2 | Acceleration setpoint yaw to DP    |
| `Measured_Wind[1]` | O     | m/s| Wind velocity in surge    |
| `Measured_Wind[2]` | O     | m/s| Wind velocity in sway    |
| `Measured_Wind[3]` | O     | rad/s| Wind velocity in yaw    |

### Thruster Dynamics

This FMU implements simplified thruster dynamics as a low pass filter which can be enabled or disabled. The thruster command is mapped directly to forces acting on the vessel without considering thruster placement. This mapping is instantaneous if thruster dynamics are disabled. It is also possible to enable or disable thrust saturation.

The FMU is based on the simulink model shown in Figure 9 and its interface is described in Table 10.

{% include figure.html
    img="/assets/img/gunnerus-dp/thruster_dynamics.png"
    num="9"
    caption="Thruster Model"
%}

**Table 10:** *Thruster Dynamics inputs and outputs (I/O).*

| Name          | I/O   | Unit | Description                                                       |
| ------------- | ----- | ---- | ----------------------------------------------------------------- |
| `Commanded_Thrust[1]`    | I     | N | Thrust demand from DP in surge |
| `Commanded_Thrust[2]`    | I     | N | Thrust demand from DP in sway |
| `Commanded_Thrust[3]`    | I     | Nm | Thrust demand from DP in yaw |
| `Thrust[1]`     | O     | N | Thrust force in surge |
| `Thrust[2]`     | O     | N | Thrust force in sway |
| `Thrust[3]`     | O     | N | Thrust force in heave (always zero)  |
| `Thrust[4]`     | O     | Nm | Thrust moment in roll (always zero) |
| `Thrust[5]`     | O     | Nm | Thrust moment in pitch (always zero) |
| `Thrust[6]`     | O     | Nm | Thrust moment in yaw |

**Table 11:** *Thruster Dynamics Parameters*

| Name          | Default   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `EnableThrustSaturation`    | True     | Enables or disable thrust force saturation    |
| `EnableThrusterDynamics`    | True     | Enables or disables thruster dynamcis    |

### Vessel Model

This FMU implements vessel dynamics. Its interface is shown in Figure 10. The sum forces input has been split into wave forces and thrust and current velocity must be provided by an external simulation model. In this case the current model FMU. Based on these inputs the FMU produces vessel velocities and accelerations in the BODY frame as well as position in the NED frame.

Additional external forces can be connected to either the wave forces or thrust port. Internally, they are both connected to the same summation block. The current velocity port only considers the x and y components of the input vector. The rest of the entries can be set to zero.
The full list of inputs and outputs are shown in Table 10

{% include figure.html
    img="/assets/img/gunnerus-dp/vessel_model.png"
    num="10"
    caption="Vessel Model"
%}

**Table 10:** *Thruster Dynamics inputs and outputs (I/O).*

| Name          | I/O   | Unit | Description                                                       |
| ------------- | ----- | ---- | ----------------------------------------------------------------- |
| `Wave_Forces[1]`    | I     | N | Wave force in surge |
| `Wave_Forces[2]`    | I     | N | Wave force in sway |
| `Wave_Forces[3]`    | I     | N | Wave force in heave |
| `Wave_Forces[4]`    | I     | Nm | Wave force in roll |
| `Wave_Forces[5]`    | I     | Nm | Wave force in pitch |
| `Wave_Forces[6]`    | I     | Nm | Wave force in yaw |
| `Thrust[1]`    | I     | N | Thrust force in surge |
| `Thrust[2]`    | I     | N | Thrust force in sway |
| `Thrust[3]`    | I     | N | Thrust force in heave |
| `Thrust[4]`    | I     | Nm | Thrust moment in roll |
| `Thrust[5]`    | I     | Nm | Thrust moment in pitch |
| `Thrust[6]`    | I     | Nm | Thrust moment in yaw |
| `Current_Velocity[1]`     | I | m/s | Current velocity in surge |
| `Current_Velocity[2]`     | I | m/s | Current velocity in sway |
| `Current_Velocity[3]`     | I | m/s | Current velocity in heave (always zero)  |
| `Current_Velocity[4]`     | I | rad/s | Current velocity in roll (always zero) |
| `Current_Velocity[5]`     | I | rad/s | Current velocity in pitch (always zero) |
| `Current_Velocity[6]`     | I | rad/s | Current velocity in yaw (always zero) |
| `Vessel_Position[1]`     | O     | m | Vessel position in north |
| `Vessel_Position[2]`     | O     | m | Vessel position in east |
| `Vessel_Position[3]`     | O     | m | Vessel position in down  |
| `Vessel_Position[4]`     | O     | m | Vessel position in roll |
| `Vessel_Position[5]`     | O     | m | Vessel position in pitch  |
| `Vessel_Position[6]`     | O     | m | Vessel position in yaw |
| `Vessel_Velocity[1]`     | O     | m/s | Vessel velocity in surge |
| `Vessel_Velocity[2]`     | O     | m/s | Vessel velocity in sway |
| `Vessel_Velocity[3]`     | O     | m/s | Vessel velocity in heave  |
| `Vessel_Velocity[4]`     | O     | rad/s | Vessel velocity in roll  |
| `Vessel_Velocity[5]`     | O     | rad/s | Vessel velocity in pitch |
| `Vessel_Velocity[6]`     | O     | rad/s | Vessel velocity in yaw |
| `Vessel_Acceleration[1]`     | O     | m/s^2 | Vessel acceleration in surge |
| `Vessel_Acceleration[2]`     | O     | m/s^2 | Vessel acceleration in sway |
| `Vessel_Acceleration[3]`     | O     | m/s^2 | Vessel acceleration in heave   |
| `Vessel_Acceleration[4]`     | O     | rad/s^2 | Vessel acceleration in roll  |
| `Vessel_Acceleration[5]`     | O     | rad/s^2 | Vessel acceleration in pitch |
| `Vessel_Acceleration[6]`     | O     | rad/s^2 | Vessel acceleration in yaw |
