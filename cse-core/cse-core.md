---
layout: default
title: "CSE"
permalink: /cse-core/cse
---

## CSE-core
The C/C++ co-simulation library orchestrates the co-simulation of models that conform to the Functional Mock-up Interface (FMI) . 
The co-simulation library includes a fixed-step master algorithm with a configurable base step size. It allows for individually defined time steps for the models, with the requirement of being a multiple of the master algorithm base step size. During simulation, there are features to observe and manipulate simulation variables either through dedicated interfaces or with a scenario runner. It is possible to apply arbitrary operations to variable values without modifying the subsystem models themselves. 
Examples include arithmetics, logical operations, transformations, unit conversions and more. This is necessity if variables connected between models must be summed or transformed by linear transformation or between coordinate frames. There are interfaces for client applications to include their own implementation of many of the included features. 

[C/C++ co-simulation library](./doxygen/0.6.0){:target="_blank"}

### CSE concepts

- Slaves
- Functions
- Manipulator
- Observer
- Modifiers

### Configuration

- Document OspSystemStructure.xml with link to .xsd
- Document OspModelDescription.xml with link to .xsd

### Scenario

A scenario file defines the simulation scenario which shall be performed. This serves for dynamic systems where actions to be made to trigger any event, activate models or sending signals to the controllers, etc. The example below shows the syntax structure of a scenario file. First is the `"description"` of what does this scenario do. 
`"action"` declares the action type which can be `"override"` , `"bias"`  or `"reset"`. `"events"` includes the `"time"` in second when the action takes place, on which `"model"`, the "`variable"` name in this model and the `"value"` for this variable (except for `"reset"` action type which doesn't require a value) .

```
{
  "description": "description of the scnario",
  "defaults": {
    "model": "model name",
    "action": "override"
  },
  "events": [
    {
      "time": event time,
      "variable": "variable name",
      "value": variable value
    },
    {
      ...      event 2       ...
    },
    {
      ...      event 3       ...
    }
  ],
  "end": scenario stop time
}
```

### Results logging

In order to log signal values from a simulation to data files, an output directory must be specified in the result folder field on the simulation setup. By
default, all signals will be logged and persisted on every sample. There will be one file generated pr. simulator. This can quickly lead to a large
amount of data being generated, so it is recommended instead specifying what signals to log using the configurable log format outlined below.
CSE supports basic configuration of specific signals to log from any simulator via an XML file. This file must be named "LogConfig.xml" (exactly
including case) and placed in the same folder as the simulators. 

```
<simulators>
    <simulator name="model name 1" decimationFactor="20">
        <variable name="variable name 1"/>
        <variable name="variable name 2"/>
    </simulator>
    <simulator name="model name 1">
        <variable name="variable name 1"/>
        <variable name="variable name 2"/>
    </simulator>
</simulators>
```

The simulators to be logged must be enclosed in a `<simulators>` tag, and each signal must specify its name in separate `<variable>` tags under each `<simulator>`. 
Leaving out any `<variable>` tags on a `<simulator>` will lead to all variables for that simulator
being logged. 
Each simulator has an optional attribute decimationFactor that specifies that simulator's decimation factor when logging variables. For example a decimation factor of 20 will lead to every 20th sample being logged. If this is
not specified, every sample will be logged. 
Leaving out a simulator from the configuration will disable logging for that simulator. The log is written in CSV format only, there is currently no support for binary or other log formats.

### Distributed co-simulation
Distributed simulation using fmu-proxy allows you to:

1. Run multiple instances of a model even though the FMU only allows it to be instantiated once.
2. Run FMUs that are not compatible with your OS or bitness.
3. Parallelize the workload onto multiple computation nodes.

Distributed simulation is currently enabled through integration with the software FMU Proxy. Both FMI 1.0 and FMI 2.0 for Co-simulation is supported.

FMU-proxy is a framework for accessing FMUs compatible with FMI for Co-simulation and Model Exchange 2.0 in a language and platform independent way. This is achieved using well established RPC technologies. Due to the technologies involved, clients and servers for FMU-proxy can be written in almost any language, on any platform!

Server implementations already exists for C++ and JVM, while client implementations exists for C++, JVM, Python and (browser) JavaScript. And its easy to add additional implementations, as the RPC frameworks will generate most of the code for you!

FMU-proxy is different from other framework for distributed FMU invocations such as DACCOSIM, FMI GO! and Coral in that it completely separates itself from the master algorithm (logically and physically). FMU-proxy is a completely standalone project which provides access to FMUs over the wire. And just that.

The idea is that other applications should use FMU-proxy whenever FMUs are required to run distributed, rather than having each application creating their own solution.

[Using fmu-proxy with CSE](./fmuproxy)


