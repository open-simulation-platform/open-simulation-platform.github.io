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
The Open Simulation Platform ([OSP](https://open-simulation-platform.com){:target="_blank"}) 
project aims to create a maritime industry ecosystem for co-simulation of “black-box” simulation models and “plug and play” configuration of systems. 
OSP relies on the Functional Mock-up Interface ([FMI](https://fmi-standard.org/){:target="_blank"}) 
standard and the new OSP interface specification ([OSP-IS](https://open-simulation-platform.com/specification/){:target="_blank"}) 
for the simulation models interfaces. This facilitates the effective building of digital twins, which in 
turn can be used to solve challenges with designing, building, integrating, commissioning and operating complex, 
integrated systems. With common tools, standards and specifications OSP seeks to foster co-simulation 
collaboration in the industry.

{% include youtubePlayer.html id=page.youtubeId %}


The software produced in the OSP JIP consists of the following elements:
- [libcosim](./libcosim) - C++ co-simulation library
- [libcosimc](./libcosim) - C wrapper for libcosim
- [cosim](./cosim) - command line interface
- [cosim demo app](./cosim-demo-app/cosim-demo-app) - demo application with a graphical user interface
- [cosim4j](./cosim4j) - libcosim java wrapper
- [osp-validator](./osp-validator) validation tools for configuration and model interfaces
- [kopl](./kopl) - co-simulation configuration tool


With OSP, simulation models are interconnected using the FMI co-simulation interface. Based on input-output variable 
mapping, the OSP master algorithm routes the data between the models. 

There is currently support for FMI 2.0 (including FMI 2.0.1) and FMI 1.0. FMI 3.0 will be supported when this standard 
becomes operational. The structure of the co-simulated system can be defined using the System Structure & Parameterization 
standard ([SSP](https://ssp-standard.org/){:target="_blank"}) 
or the new [OSP System Structure](/libcosim/configuration) format that enables the use of OSP-IS. 

The main benefit introduced with OSP-IS is the possibility to define FMU interconnections at a higher level, i.e. variable 
group connections rather than scalar connections. Additionally, it enables automatic unit conversions and provides the a 
priori validation of the system configuration utilizing an ontology that models the connection types. 

OSP comes with [example configurations](/demo-cases) containing FMUs and co-simulation configuration exemplified using both OSP-IS and 
SSP.
