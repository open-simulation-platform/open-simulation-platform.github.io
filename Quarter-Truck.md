---
layout: default
title: "cse-demo-app"
permalink: /Quarter-Truck
---

## CSE demo application - Quarter-Truck

### System Description

This example describes a low-level test case for the performance of the cse in terms of accuracy and efficiency. 
A quarter truck model is developed as illustrated below. 
![foo](/assets/img/QuarterTruck.png "Figure 1")
    
### Model Description

Three FMUs are generated namely chassis, wheel and ground. The ground and chassis each have one bond for connection, the wheel has two bonds that are commutative to those of the ground and chassis.  
The following parameters are used:

| Name        | Parameter      |Default Value        |
| :---           |:---:         | :---:           |
| Wheel Mass        | mWheel      | 40 kg       |
| Chassis Mass        | mChassis     | 400 kg        |
| Wheel Spring Stiffness      | kWheel      | 150000 Nm^-1     |
| Wheel Damper Coefficient        | dWheel     | 0 Nsm^-1      |
| Chassis Spring Stiffness         | kChassis     | 15000 Nm^-1       |
| Chassis Damper Coefficient         | dChassis     |  1000 Nsm^-1      |

### Simulation Scenario 
The ground profile is defined as a step function. Starting from equilibrium state, the ground profile is excited by the jump 0.1m in z-direction at 1s. Figure 2 shows the displacements of the masses.  
![foo](/assets/img/Position.png "Figure 2")
![foo](/assets/img/PositionCosim.png "Figure 3")