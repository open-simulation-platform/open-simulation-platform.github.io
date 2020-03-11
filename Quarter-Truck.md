---
layout: default
title: "cse-demo-app"
permalink: /Quarter-Truck
---

## CSE demo application - Quarter-Truck

### System Description

This example describes a low-level test case for the performance of the cse in terms of accuracy and efficiency. 
Figure 1 shows a simplified model of a quarter truck with two masses mWheel and mChassis representing the mass of wheel and chassis respectively. Both masses have single vertical degree of freedom and coupled by linear spring-damper systems representing the suspension force and tire force. The ground profile is give as external input. 

![foo](/assets/img/QuarterTruckFig1.png "Figure 1")
    
### Model Description

For co-simulation, three FMUs are generated namely the chassis, wheel and ground. The ground and chassis each have one variable group for connection to the wheel which has two variable groups that are commutative to those of the ground and chassis. Table 1 shows the connections for these variables.
The local parameters in the model are listed in Table 2.

**Table 1:** *Input and output variables of the quarter truck fmus for connection*

| Output Variable     |Input Variable  |
|:---:         | :---:           |
| *chassis.p.e*     | *wheel.p.e*      |
| *wheel.p.f*   | *chassis.p.f*        |
| *wheel.p.e*     | *ground.p.e*     |
| *ground.p.f*     | *wheel.p.f*     |


**Table 2:** *Local parameters of the quarter truck model*

| Name        | Parameter      |Default Value        |
| :---           |:---:         | :---:           |
| Wheel Mass        | *mWheel*      | 40 kg       |
| Chassis Mass        | *mChassis*     | 400 kg        |
| Wheel Spring Stiffness      | *kWheel*      | 150000 Nm^-1     |
| Wheel Damper Coefficient        | *dWheel*     | 0 Nsm^-1      |
| Chassis Spring Stiffness         | *kChassis*     | 15000 Nm^-1       |
| Chassis Damper Coefficient         | *dChassis*     |  1000 Nsm^-1      |

### Simulation Results 
The following scenario is performed as example. The system starts from equilibrium state where *zWheel* = *zChassis* = 0. The ground profile is defined as a step function excited by a jump of 0.1m in vertical direction at 1s. Figure 2 and Figure 3 show the vertical displacements of the wheel and the chassis. We accept the result using very fine time step (0.001s) as the benchmark and increased time step (0.01s) for co-simulation. The accuracy can be improved by applying advanced master algorithms provided variable co-simulation time steps, and optionally th support of setting/getting derivatives, getting/setting states. In order to enforce reliable macro-step control algorithms, error estimation is needed. Classic Richardson extrapolation and a modified error estimator is described in [1]. Figure 4 show the results of using an energy-based algorithm (ECCO) for error correction [2].     


<img src="/assets/img/QuarterTruckFig2.png" width="80%" > 
<img src="/assets/img/QuarterTruckFig3.png" width="200" > 
<img src="/assets/img/QuarterTruckFig4.png" > 


### Simulation using cse-demo-app 
User guide of the cse demo application, i.e., cse-server-go can be found [cse-demo-app](./cse-demo-app.md). Figure 5 shows the demo application set up and Figure 6 shows the time series of the simulation output.

![foo](/assets/img/QuarterTruckFig5.png  "Figure 5")
![foo](/assets/img/QuarterTruckFig6.png  "Figure 6")

### References 
[1] Arnold, M., Clauss, C., & Schierz, T. (2014). Error analysis and error estimates for co-simulation in FMI for model exchange and co-simulation V2. 0. In Progress in Differential-Algebraic Equations (pp. 107-125). Springer, Berlin, Heidelberg.

[2] Sadjina, S., Kyllingstad, L. T., Rindarøy, M., Skjong, S., Æsøy, V., and Pedersen, E. (September 12, 2018). "Distributed Co-simulation of Maritime Systems and Operations." ASME. J. Offshore Mech. Arct. Eng. February 2019; 141(1): 011302. https://doi.org/10.1115/1.4040473