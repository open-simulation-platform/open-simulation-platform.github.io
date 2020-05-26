---
layout: default
title: "libcosim"
permalink: /libcosim
has_children: true
has_toc: false
nav_order: 1
---

<div style="text-align: right">
    <b>
        <a href="https://github.com/open-simulation-platform/libcosim">libcosim</a>
        and  
        <a href="https://github.com/open-simulation-platform/libcosimc">libcosimc</a>   
        on GitHub
    </b>
</div>

# libcosim
The OSP co-simulation C/C++ libraries that orchestrates the co-simulation of models that conform to the Functional Mock-up Interface (FMI). 
`libcosim` is the C++ API and `libcosimc` is a C wrapper for a subset of `libcosim`. 

The co-simulation library includes a fixed-step master algorithm with a configurable base step size. It allows for individually defined time steps for the models, with the requirement of being a multiple of the master algorithm base step size. During simulation, there are features to observe and manipulate simulation variables either through dedicated interfaces or with a scenario runner. It is possible to apply arbitrary operations to variable values without modifying the subsystem models themselves. 
Examples include arithmetics, logical operations, transformations, unit conversions and more. This is necessity if variables connected between models must be summed or transformed by linear transformation or between coordinate frames. There are interfaces for client applications to include their own implementation of many of the included features. 

Detailed API documentation is found here:
[`libcosim`](/libcosim/doxygen/libcosim/0.7.0){:target="_blank"}
[`libcosimc`](/libcosim/doxygen/libcosimc/0.7.0){:target="_blank"}.

## libcosim concepts
- Slaves
- Functions
- Manipulator
- Observer
- Modifiers

![foo](/assets/img/cseFig2.png "Figure 2")







