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

See below for ways to load an FMU using proxyfmu.

##### OspSystemStructure.xml

```xml
<Simulators>
    <Simulator name="FMU1"
        source="proxyfmu://localhost?file=path/to/fmu1.fmu"/>
    <Simulator name="FMU2"
        source="proxyfmu://127.0.0.1:9090?file=path/to/fmu2.fmu"/>
here>"/>
</Simulators>
```

##### SystemStructure.ssp

```xml
<ssd:Elements>
    <ssd:Component name="FMU1"
        source="proxyfmuy://localhost?file=path/to/fmu1.fmu"/>
    <ssd:Component name="FMU2"
        source="proxyfmu://127.0.0.1:9090?file=path/to/fmu1.fmu"/>
here>"/>
</ssd:Elements>
```

When choosing `localhost` as the hostname and omitting the port, `libcosim` handles spawning new processes for the models to run. Just make sure that the `proxyfmu`executable is available. It could either be located in the current directory or in the directory of the executable that uses it (e.g. `cosim` or `cosim demo app`).
However, it is also possible to run models on a different PC. To accomplish this, start `proxyfmu_booter` on the target PC with `--port <arg>` as an argument. Then supply the IP and port number to the configuration. 

The [Demo Cases](/demo-cases) include configuration for distributed co-simulation with proxyfmu, and this 
[section](/cosim-demo-app/user-guide#distributed-co-simulation-using-proxyfmu) shows how it's done with the `cosim demo app`.

