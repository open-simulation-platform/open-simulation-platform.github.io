---
layout: default
title: "CSE"
permalink: /
---

## Core Simulation Environment
The Core Simulation Environment (CSE) is an open source software for co-simulation, built on a combination of technical solutions from the Open Simulation Platform (OSP) initiators and other established industrial solutions. The key working principle has been to use the best available technology and knowledge to meet the requirements for the OSP to efficiently serve its purpose. The CSE source code will be made available on GitHub, free of charge, through the MPL 2.0 open source licence.
The open-source software coined the Core Simulation Environment (CSE) produced in the OSP JIP consists of the following elements:

- [CSE core](./cse-core/cse) --> Download [cse-core v0.6.0](https://osp-jenkins.azurewebsites.net/job/open-simulation-platform/)
- [CSE demo application](./cse-demo-app/cse-demo-app) --> Download [cse-server-go v0.6.0](https://osp-jenkins.azurewebsites.net/job/open-simulation-platform/)
- [Command-line interface](./cse-cli)
- [Model interface validator](./model-interface-validator)
- [CSE Java wrapper](./cse-java-wrapper)

The conceptual architecture of the co-simulation is illustrated as in figure below. The models are connected using the standardized Functional Mock-up Interface (FMI) co-simulation interface. Based on the input-output variable mapping included in the configuration file, the OSP master algorithm routes the data between the models. 

<figure>
<img src="/assets/img/cseFig1.png" width="500"> 
</figure>

Currently, the OSP is supporting [FMI 2.0 and FMI 1.0](https://fmi-standard.org/) and will be updated to support FMI 3.0 when this standard becomes operational. 
The structure of the co-simulated system can be defined using the System Structure & Parameterization (SSP) standard or the new model interface standard MSMI (Marine System Model Interface) developed in the OSP project. 
Both standards can be used to include Functional Mock-up Unit (FMUs), defining their interconnections, initial values for simulation variables and individual model step sizes. 
The benefit introduced with MSMI is the possibility to defining FMU interconnections at a higher level, i.e. variable group connections rather than scalar connections. 
Additionally, it enables automatic unit conversions and provides the a priori validation of the system configuration utilizing an ontology that models the connection types. 
CSE comes with example configurations containing FMUs and co-simulation configuration exemplified in both MSMI and SSP.
