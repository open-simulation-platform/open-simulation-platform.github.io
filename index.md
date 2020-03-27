---
layout: default
title: "CSE"
permalink: /
---

## Core Simulation Environment

The conceptual architecture of the co-simulation is illustrated as in figure below. The models are connected using the standardized FMI co-simulation interface. Based on the input-output variable mapping included in the configuration file, the OSP master algorithm routes the data between the models. 

<figure>
<img src="/assets/img/cseFig1.png" width="500"> 
</figure>

Currently, the OSP is supporting FMI 2.0 and FMI 1.0 and will be updated to support FMI 3.0 when this standard becomes operational. 
The structure of the co-simulated system can be defined using the System Structure & Parameterization (SSP) standard or the new model interface standard MSMI (Marine System Model Interface) developed in the OSP project. 
Both standards can be used to include FMUs, defining their interconnections, initial values for simulation variables and individual model step sizes. 
The benefit introduced with MSMI is the possibility to defining FMU interconnections at a higher level, i.e. variable group connections rather than scalar connections. 
Additionally, it enables automatic unit conversions and provides the a priori validation of the system configuration utilizing an ontology that models the connection types. 
CSE comes with example configurations containing FMUs and co-simulation configuration exemplified in both MSMI and SSP.


### CSE core
The Core Simulation Environment (CSE) is an open source software for co-simulation, built on a combination of technical solutions from the OSP initiators and other established industrial solutions. The key working principle has been to use the best available technology and knowledge to meet the requirements for the OSP to efficiently serve its purpose. The CSE source code will be made available on GitHub, free of charge, through the MPL 2.0 open source licence.
The open-source software coined the Core Simulation Environment (CSE) produced in the OSP JIP consists of the following elements:

- C/C++ co-simulation library --> Download [cse-core v0.6.0](./cse-core)
- Demo application --> Download [cse-server-go v0.6.0](./cse-server-go)
- Command-line interface
- Model interface validator
- CSE Java wrapper

[more details](./cse-core/cse)

#### CSE C/C++ co-simulation library
The C/C++ co-simulation library orchestrates the co-simulation of models that conform to the Functional Mock-up Interface (FMI) . 
The co-simulation library includes a fixed-step master algorithm with a configurable base step size. It allows for individually defined time steps for the models, with the requirement of being a multiple of the master algorithm base step size. During simulation, there are features to observe and manipulate simulation variables either through dedicated interfaces or with a scenario runner. It is possible to apply arbitrary operations to variable values without modifying the subsystem models themselves. 
Examples include arithmetics, logical operations, transformations, unit conversions and more. This is necessity if variables connected between models must be summed or transformed by linear transformation or between coordinate frames. There are interfaces for client applications to include their own implementation of many of the included features. 

[more details](./cse-core/doxygen/0.6.0){:target="_blank"}

#### CSE demo application
The demo application is developed to showcase how the CSE co-simulation library can be used in an application and to provide a graphical user interface to manage co-simulations. It allows for loading configuration, controlling the co-simulation and easy exploration of the co-simulation library’s features. There are options for real time execution or simulation in free mode (as fast as the hardware allows). During simulation, the user can observe simulation variables, manipulate these and visualise the simulation with plotting features. 
A scenario management feature allows change of the simulation variables automatically based on a scenario file. It is possible to log the simulation variables of all FMUs in dedicated data files based on predefined rules.

[more details](./cse-demo-app/cse-demo-app)

#### CSE CLI
For the cases where a graphical user interface is not available or needed, the CSE comes as a command line application. 

[more details](./cse-cli)

#### Model interface validator
The Model interface validator is a tool to verify that the simulation model complies to MSMI. 
The validator loads the OSPModelDescription.xml and the ontology, and checks that the configuration is correct. It can be used to verify that a given FMU with an OSPModelDescription.xml complies with MSMI. Or, it can be used to verify that the connections between two FMUs are semantically correct.

[more details](./model-interface-validator)

#### CSE Java wrapper
The Java wrapper enables Java applications to make use of the co-simulation library.

