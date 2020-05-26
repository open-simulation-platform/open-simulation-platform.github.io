---
layout: default
title: "House"
parent: "Demo Cases"
nav_order: 3
has_toc: false
permalink: /cosim-demo-app/House
---

# House

This demo case simulates the temperature deviations of a house which consists of two rooms.  

## Model Description
A simplified model represents the heat exchange dynamics of a house as shown in Figure 1.  

![foo](/assets/img/HouseFig1.png "Fig.1 A simplified house model")

**Table 1:** *List of FMUs of the house configuration and their input/output variables for connection.*

| FMU Name      | Local Parameters    | Input      | Output        |
| :---          |:---:                | :---:      | :---:         |
| Clock         | `Reset`               | -          | `Clock`      |
| InnerWall     |`k`            | `T_room1`    | `h_wall`        |
|     |           |  `T_room2`     |       |
| OuterWall1     |   `k_outsidewall`       | `T_outside`     | `h_wall`        |
|        |         | `T_room1`       |       |
| OuterWall2     |   `k_outsidewall`       | `T_outside`    | `h_wall`        |
|        |         | `T_room2`       |       |
| Room1    |   `Tinit_Room1`     | `TRoomReset`    | `T_room`       |
|        |         | `h_InnerWall`     |       |
|        |         |`h_OuterWall`    |       |
|        |         | `h_powerHeater`    |       |
| Room2    |   `Tinit_Room2`     | `TRoomReset`    | `T_room`       |
|        |         | `h_InnerWall`     |       |
|        |         | `h_OuterWall`    |       |
|        |         | `h_powerHeater`    |       |
| TempController    |   `OvenHeatTransfer`     | `T_clock`    |`h_room1`      |
|        |    `T_heatStart`     | `T_room1`     |    `h_room2`   |
|        |    `T_heatStop`     | `T_room2`    |    `roomStates`   |
|        |    `transferTime`     |    |       |

Figure 2 shows the simulation results of the temperature of each room over time.
![foo](/assets/img/HouseFig3.png "Simulation results of Room1 and Room2 temperature")

More information on how to use the _cosim demo app_ is available in the [user guide](./user-guide), where the [DP-Ship](./DPShip) demo test case was used as an example.