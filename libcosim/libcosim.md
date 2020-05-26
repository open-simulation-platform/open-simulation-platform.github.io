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

The co-simulation library includes a fixed-step master algorithm with a configurable base step size. 
It allows for individually defined time steps for the models (with the requirement of being a multiple of the master algorithm base step size).
 
It is possible to apply arbitrary operations to variable values without modifying the models. 
Examples include arithmetics, logical operations, transformations, unit conversions and more. 
This is necessary if variables connected between models must be summed or even modified through a linear or a coordinate frame transformation, read more in [simulation configuration](./configuration) 

During simulation, there are features to observe and manipulate simulation variables either through dedicated interfaces or with the help of a [scenario runner](./scenario).

There is also support for [logging](./logging) the simulation results and for running [distributed co-simulation](./fmuproxy).
There are interfaces for client applications to include their own implementation of many of the included features. 

Detailed API documentation is found here:
[`libcosim`](/libcosim/doxygen/libcosim/0.7.0){:target="_blank"}
[`libcosimc`](/libcosim/doxygen/libcosimc/0.7.0){:target="_blank"}.

## Architecture
The figure below shows a schematic of the main classes and interfaces in libcosim and how they hang together.

The top part shows classes that are typically used in the pre-simulation phase.
These can be used to set up the system model in an "offline" manner,
without loading any external model code or connecting to remote simulators.
The central class here is [`system_structure`](/libcosim/doxygen/libcosim/0.7.0/classcosim_1_1system__structure.html){:target="_blank"},
which, as the name suggests, contains the entire system structure.

The lower part shows classes that are used in the simulation phase.
Here, the central one is [`execution`](/libcosim/doxygen/libcosim/0.7.0/classcosim_1_1execution.html){:target="_blank"},
which represents and controls a single run, or _execution_, of the simulation.
It offers several customisation points in the form of interfaces that can be implemented by classes which perform specific tasks during a simulation.
These are:

  - `algorithm`: Controls the step size and the routing of variable values between `simulators` and `functions`. Typical uses: Generic or special-purpose co-simulation algorithms.
  - `slave`, `async_slave`: A component of the simulated system. Typical uses: Mathematical models, hardware interfaces, interfaces to external software.
  - `function`: An operation performed _between_ time steps. Typical uses: Transformations, signal arithmetic, logical operations.
  - `observer`: An entity that has a read-only view of the simulation. Typical uses: Logging, visualisation, real-time postprocessing.
  - `manipulator`: An entity that can modify certain aspects of the simulation, such as simulator input variables. Typical uses: Scenario control, signal modification.

Each `execution` can only have one `algorithm`, but it can contain any number of the others.

![A diagram that shows the main classes and interfaces in libcosim and the relationships between them](/assets/img/libcosim-architecture.png "libcosim architecture")
