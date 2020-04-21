---
layout: default
title: "CSE Demo App"
has_children: true
permalink: /cse-demo-app/cse-demo-app
---

## CSE demo application

The demo application is developed to showcase how the CSE co-simulation library can be used in an application and to provide a graphical user interface to manage co-simulations. It allows for loading configuration, controlling the co-simulation and easy exploration of the co-simulation library’s features. There are options for real time execution or simulation in free mode (as fast as the hardware allows). During simulation, the user can observe simulation variables, manipulate these and visualise the simulation with plotting features. 
A scenario management feature allows change of the simulation variables automatically based on a scenario file. It is possible to log the simulation variables of all FMUs in dedicated data files based on predefined rules.


The intention of this application is to provide a user interface for the cse-core. It demonstrates the features of the cse-core. Basic
co-simulation functionality is demonstrated with a simple graphical user interface:
- Load co-simulation configuration
- Simulation control
- Observe simulation variables
- Override/set simulation variables
- Running scenarios
- Plotting variables
- Logging results to file

A step by step user guide is provided on how to use the cse demo application.
We use one of the demo test cases (dp-ship) to show the process. The demo cases can be downloaded at the end of this page.

### [User guide](./user-guide)



### Demo co-simulation case(s)

- [DPShip](./DPShip)
- [House](./House)
- [Quarter-Truck](./Quarter-Truck)

#### Demo simulation models
The above demo models can be downloaded from [here](https://github.com/open-simulation-platform/cse-demos).