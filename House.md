---
layout: default
title: "cse-demo-app"
permalink: /House
---

## CSE demo application - House
This file contains a minimal description of the House demo case.

### Introduction
This demo case simulates the temperature deviations of a house which consists of two rooms.  

### Model Description
A simplified model represents the heat exchange dynamics of a house as shown in Figure 1.  
![foo](/assets/img/HouseFig1.png "Figure 1")

Table 1 gives the fmus and their interfaces for connections.  

| FMU Name      | Local Parameters    | Input      | Output        |
| :---          |:---:                | :---:      | :---:         |
| Clock         | Reset               | -          | Clock      |
| InnerWall     |k            | T_room1    | h_wall        |
|     |           |  T_room2     |       |
| OuterWall1     |   k_outsidewall       | T_outside     | h_wall        |
|        |         | T_room1       |       |
| OuterWall2     |   k_outsidewall       | T_outside     | h_wall        |
|        |         | T_room2       |       |
| Room1    |   Tinit_Room1     | TRoomReset    | T_room       |
|        |         | h_InnerWall     |       |
|        |         | h_OuterWall    |       |
|        |         | h_powerHeater    |       |
| Room2    |   Tinit_Room2     | TRoomReset    | T_room       |
|        |         | h_InnerWall     |       |
|        |         | h_OuterWall    |       |
|        |         | h_powerHeater    |       |
| TempController    |   OvenHeatTransfer     | T_clock    |h_room1      |
|        |    T_heatStart     | T_room1     |    h_room2   |
|        |    T_heatStop     | T_room2    |    roomStates   |
|        |    transferTime     |    |       |
### Simulation using the cse-demo-app

User guide of the cse demo application, i.e., cse-server-go can be found [cse-demo-app](./cse-demo-app.md). Figure 2 shows the demo application set up. Figure 3 and Figure 4 shows the time series of the simulation output, specifically, the room temperatures and their variations.

![foo](/assets/img/HouseFig2.png "Figure 2")
![foo](/assets/img/HouseFig3.png "Figure 3")
![foo](/assets/img/HouseFig4.png "Figure 4")

### References
