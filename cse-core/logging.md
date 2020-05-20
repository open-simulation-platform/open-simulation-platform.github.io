---
layout: default
title: "Results logging"
permalink: /cse-core/logging
has_toc: false
parent: "libcosim"
nav_order: 3
---
# Results logging

In order to log signal values from a simulation to data files, an output directory must be specified in the result folder field on the simulation setup. By
default, all signals will be logged and persisted on every sample. There will be one file generated pr. simulator. This can quickly lead to a large
amount of data being generated, so it is recommended instead specifying what signals to log using the configurable log format outlined below.
`libcosim` supports basic configuration of specific signals to log from any simulator via an XML file. This file must be named `LogConfig.xml` (exactly
including case) and placed in the same folder as the simulators. 

| sub-element     | attribute | Description                                                                                                                              |
| :--------------- | :------------| :-------------------------------------------------------------------------------------------------------------------------------------- |
| <*simulator*>          | `name`  | Contains all variables to be logged, attribute `name` declares of the name of the variable.
|         | `decimationFactor`  | attribute `decimationFactor` declares the sampling rate of the simulation results. For example, decimationFactor=1 means the results of every simulation step of the simulator are logged. And decimationFactor=10 means every 10th of the simulation results are logged. |
| <*variable*>        | `name` | Declare the name of the variable.

```xml
<simulators>
    <simulator name="model1" decimationFactor="10">
        <variable name="variable1"/>
        <variable name="variable2"/>
    </simulator>
    <simulator name="model2">
        <variable name="variable1"/>
        <variable name="variable2"/>
    </simulator>
</simulators>
```

The simulators to be logged must be enclosed in a `<simulators>` tag, and each signal must specify its name in separate `<variable>` tags under each `<simulator>`. 
Leaving out any `<variable>` tags on a `<simulator>` will lead to all variables for that simulator
being logged. 
Each simulator has an optional attribute decimationFactor that specifies that simulator's decimation factor when logging variables. For example a decimation factor of 20 will lead to every 20th sample being logged. If this is
not specified, every sample will be logged. 
Leaving out a simulator from the configuration will disable logging for that simulator. The log is written in CSV format only, there is currently no support for binary or other log formats.
