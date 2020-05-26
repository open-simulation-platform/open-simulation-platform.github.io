---
layout: default
title: "Home"
permalink: /
has_toc: false
youtubeId: E5KumnkeKO8
---

<div style="text-align: right">
    <b>
        <a href="https://github.com/open-simulation-platform">OSP</a> on GitHub
    </b>
</div>

# Open Simulation Platform Software
The open source software for co-simulation is built on a combination of technical solutions from the 
Open Simulation Platform (OSP) initiators and other established industrial solutions. 
The key working principle has been to use the best available technology and knowledge to meet the requirements for 
the OSP to efficiently serve its purpose. The source code is made available on 
[GitHub](https://github.com/open-simulation-platform), free of charge, through the MPL 2.0 open source licence.

{% include youtubePlayer.html id=page.youtubeId %}


The software produced in the OSP JIP consists of the following elements:
- [libcosim](./libcosim) - C++ co-simulation library
- [libcosimc](./libcosim) - C wrapper for libcosim
- [cosim](./cosim) - command line interface
- [cosim demo app](./cosim-demo-app/cosim-demo-app) - demo application with a graphical user interface
- [cosim4j](./cosim4j) - libcosim java wrapper
- [osp-validator](./model-interface-validator) validation tools for configuration and model interfaces
- [kopl](./kopl) - co-simulation configuration tool


With OSP, simulation models are interconnected using the Functional Mock-up Interface ([FMI](https://fmi-standard.org/)) 
co-simulation interface. Based on input-output variable mapping, the OSP master algorithm routes the data between the 
models. 

There are currently support for FMI 2.0 and FMI 1.0 and FMI 3.0 will be supported when this standard becomes 
operational. The structure of the co-simulated system can be defined using the System Structure & Parameterization 
standard ([SSP](https://ssp-standard.org/)) or the new model interface standard OSP Interface Specification 
([OSP-IS]()) developed in the OSP project. 

The benefits introduced with OSP-IS is the possibility to define FMU interconnections at a higher level, i.e. variable 
group connections rather than scalar connections. Additionally, it enables automatic unit conversions and provides the a 
priori validation of the system configuration utilizing an ontology that models the connection types. 

OSP comes with example configurations containing FMUs and co-simulation configuration exemplified using both OSP-IS and 
SSP.
