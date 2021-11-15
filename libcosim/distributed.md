---
layout: default
title: "Distributed co-simulation"
permalink: /libcosim/distributed
has_toc: false
parent: "Co-simulation C/C++ libraries"
nav_order: 4
---
# Distributed co-simulation
`libcosim` can run distributed co-simulation which allows you to:

1. Run multiple instances of a model even though the FMU only allows it to be instantiated once.
2. Run FMUs that are not compatible with your OS or bitness.
3. Parallelize the workload onto multiple computation nodes.

Distributed simulation is currently enabled through integration with the software [proxyfmu](https://github.com/open-simulation-platform/proxy-fmu). 
Both FMI 1.0 and FMI 2.0 for Co-simulation is supported.

The [Demo Cases](/demo-cases) include configuration for distributed co-simulation with proxyfmu, and this 
[section](/cosim-demo-app/user-guide#distributed-co-simulation-using-proxyfmu) shows how it's done with the `cosim demo app`.
