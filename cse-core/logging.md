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
The `Cosim Demo App` supports basic configuration of specific signals to log from any simulator via an XML file. This file must be named `LogConfig.xml` (exactly
including case) and placed in the same folder as the configuration file (OspSystemStructure.xml or SystemStructure.ssd). 

| sub-element     | attribute | Description                                                                                                                              |
| :--------------- | :------------| :-------------------------------------------------------------------------------------------------------------------------------------- |
| <*simulator*>          | `name`  | Contains all variables to be logged, attribute `name` declares of the name of the simulator.
|         | `decimationFactor`  | Optional attribute `decimationFactor` declares the sampling rate of the simulation results. For example, decimationFactor=1 means the results of every simulation step of the simulator are logged. And decimationFactor=10 means every 10th of the simulation results are logged. |
| <*variable*>        | `name` | Declares the name of the variable.

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