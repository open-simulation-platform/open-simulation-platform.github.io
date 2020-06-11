---
layout: default
title: "Maritime Reference Models"
has_children: true
permalink: /demo-cases
nav_order: 5
has_toc: false
---
<div style="text-align: right">
    <b>
        <a href="https://github.com/open-simulation-platform/demo-cases">demo-cases</a>   
        on GitHub
    </b>
</div>

# Maritime Reference Models

This page contains a list of marine reference models (Table 1) and [example configurations](#example-configurations) using these models.

**Table 1:** *List of maritime reference models.*

| Component | Tool/code | OS | Architecture | Used in |
| --------- | --------- | -- | ------------ | ------------------ | ------- |
| Winch Actuator Model | 20-sim | Windows | 32/64bit | [Launch and Recovery System](https://github.com/open-simulation-platform/demo-cases/tree/master/lars) |
| Winch model | SimulationX | Windows | 64bit | [Launch and Recovery System](https://github.com/open-simulation-platform/demo-cases/tree/master/lars)
| Winch model + controller | Simulink | Windows | 64bit | [Construction Vessel](https://github.com/open-simulation-platform/demo-cases/tree/master/construction-vessel)
| A-frame model (geometric) | 20-sim | Windows | 32/64bit | [Launch and Recovery System](https://github.com/open-simulation-platform/demo-cases/tree/master/lars) |
| A-frame actuator model | 20-sim | Windows | 32/64bit | [Launch and Recovery System](https://github.com/open-simulation-platform/demo-cases/tree/master/lars) |
| Crane model | 20-sim | Windows | 64bit | [Gunnerus Palfinger Crane](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-palfinger-crane)
| PMAzimuth thruster | Vesim | Windows | 64bit | [Gunnerus Palfinger Crane](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-palfinger-crane) and [Gunnerus Path-following](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-waypoint-following)
| Thruster drive | 20-sim | Windows | 64bit | [Gunnerus Palfinger Crane](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-palfinger-crane) and [Gunnerus Path-following](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-waypoint-following)
| Simple thruster model | Simulink | Windows | 64bit | [Gunnerus-DP](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-dp)
| Generic thruster model | Open Modelica | Windows | 64 bit | None
| Set of thruster models | Simulink | Windows | 64bit | [Construction vessel](https://github.com/open-simulation-platform/demo-cases/tree/master/construction-vessel)
| Vessel model, Gunnerus (hull) | Simulink | Windows | 64bit | [Gunnerus-DP](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-dp)
| Vessel model, construction vessel (hull) | Simulink | Windows | 64bit | [Construction Vessel](https://github.com/open-simulation-platform/demo-cases/tree/master/construction-vessel)
| Vessel model, Gunnerus (hull + environment) | Vesim | Windows | 64bit | [Gunnerus Palfinger Crane](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-palfinger-crane) and[ Gunnerus Path-following](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-waypoint-following)
| Vessel model (hull + environment + thrusters) | 20-sim | Windows | 64bit | [DP-Ship](https://github.com/open-simulation-platform/demo-cases/tree/master/dp-ship)
| Current model | Simulink | Windows | 64bit | [Gunnerus-DP](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-dp)
| Wind model | Simulink | Windows | 64bit | [Construction vessel](https://github.com/open-simulation-platform/demo-cases/tree/master/construction-vessel)
| GNSS sensor model | C++ | Windows/Linux | 64bit | [Sensor and communication configurations](https://github.com/open-simulation-platform/demo-cases/tree/master/sensors-and-senders)
| Gyro sensor model | C++ | Windows/Linux | 64bit | [Sensor and communication configurations](https://github.com/open-simulation-platform/demo-cases/tree/master/sensors-and-senders)
| Wind sensor model | C++ | Windows/Linux | 64bit | [Sensor and communication configurations](https://github.com/open-simulation-platform/demo-cases/tree/master/sensors-and-senders)
| VRU sensor model | C++ | Windows/Linux | 64bit | [Sensor and communication configurations](https://github.com/open-simulation-platform/demo-cases/tree/master/sensors-and-senders)
| Power plant model | 20-sim | Windows | 64bit | [Gunnerus Palfinger Crane](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-palfinger-crane) and [Gunnerus Path-following](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-waypoint-following)
| 2-split diesel electric power system | Simulink | Windows | 64bit | [Construction Vessel](https://github.com/open-simulation-platform/demo-cases/tree/master/construction-vessel)
| DP controller | Simulink | Windows | 64bit | [Construction Vessel](https://github.com/open-simulation-platform/demo-cases/tree/master/construction-vessel)
| DP controller | C++ | Windows/Linux | 64bit | [DP-Ship](https://github.com/open-simulation-platform/demo-cases/tree/master/dp-ship)
| Simple DP controller | Simulink | Windows | 64bit | [Gunnerus-DP](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-dp)
| Trajectory controller | Java | Windows/Linux | 64bit | [Gunnerus Path-following](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-waypoint-following)
| WaypointProvider2DOF | Java | Windows/Linux | 64bit | [Gunnerus Path-following](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-waypoint-following)
| DP controller | Java | Windows/Linux | 64bit | [Gunnerus Palfinger Crane](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-palfinger-crane) 
| DP allocator | Simulink | Windows | 64bit | [Construction Vessel](https://github.com/open-simulation-platform/demo-cases/tree/master/construction-vessel)
| DP allocator for fixed angles | Java | Windows | 64bit | [Gunnerus Palfinger Crane](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-palfinger-crane) 
| Thrust allocation | C++ | Windows/Linux | 64bit | [DP-Ship](https://github.com/open-simulation-platform/demo-cases/tree/master/dp-ship)
| WaypointProvider3DOF | Java | Windows/Linux | 64bit | [Gunnerus Palfinger Crane](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-palfinger-crane) 
| Reference generator | C++ | Windows/Linux | 64bit | [Construction Vessel](https://github.com/open-simulation-platform/demo-cases/tree/master/construction-vessel)
| Reference model | Simulink | Windows | 64bit | [Gunnerus Palfinger Crane](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-palfinger-crane) 
| Nonlinear passive observer | Java | Windows/Linux | 64bit | [DP-Ship](https://github.com/open-simulation-platform/demo-cases/tree/master/dp-ship)
| Nonlinear passive observer | 20-sim | Windows | 64bit | [Gunnerus-DP](https://github.com/open-simulation-platform/demo-cases/tree/master/gunnerus-dp)
| Generic interface FMU | C++ | Windows/Linux | 64bit | [Sensor and communication configurations](https://github.com/open-simulation-platform/demo-cases/tree/master/sensors-and-senders)
| GNSS NMEA interface FMU (GGA + GST) | C++ | Windows/Linux | 64bit | [Sensor and communication configurations](https://github.com/open-simulation-platform/demo-cases/tree/master/sensors-and-senders)
| Gyro NMEA interface FMU (HEDTH) | C++ | Windows/Linux | 64bit | [Sensor and communication configurations](https://github.com/open-simulation-platform/demo-cases/tree/master/sensors-and-senders)
| Wind measurement NMEA interface FMU (MWV) | C++ | Windows/Linux | 64bit |[Sensor and communication configurations](https://github.com/open-simulation-platform/demo-cases/tree/master/sensors-and-senders)
| VRU NMEA interface (SXN) | C++ | Windows/Linux | 64bit | [Sensor and communication configurations](https://github.com/open-simulation-platform/demo-cases/tree/master/sensors-and-senders)

## Example configurations

Demonstration cases including simulation models and configuration with OSP-IS and SSP:

- [Gunnerus-DP](./cosim-demo-app/Gunnerus-DP)
- [DPShip](./cosim-demo-app/DPShip)
- [Quarter-Truck](./cosim-demo-app/Quarter-Truck)
- [House](./cosim-demo-app/House)
- [Open Modelica Thruster](./cosim-demo-app/Open-Modelica-Thruster)
- [Launch and Recovery System](./cosim-demo-app/lars)
- [Gunnerus Path-following](./cosim-demo-app/gunnerus-path-following)
- [Construction Vessel](./cosim-demo-app/Construction-Vessel)
