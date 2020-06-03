---
layout: default
title: "Results logging"
permalink: /libcosim/logging
has_toc: false
parent: "Co-simulation C/C++ libraries"
nav_order: 3
---
# Results logging

By default, all signals will be logged and persisted on every sample. There will be one file generated pr. simulator. This can quickly lead to a large
amount of data being generated, so it is recommended instead specifying what signals to log using the configurable log format outlined below.
`libcosim` supports basic configuration of specific signals to log from any simulator via an XML file. This file must be named `LogConfig.xml` (exactly
including case) and placed in the same folder as the configuration file (`OspSystemStructure.xml` or `SystemStructure.ssd`). 

| sub-element     | attribute | Description                                                                                                                              |
| :--------------- | :------------| :-------------------------------------------------------------------------------------------------------------------------------------- |
| <*simulator*>          | `name`  | Contains all variables to be logged, attribute `name` declares of the name of the simulator.
|         | `decimationFactor`  | Optional attribute `decimationFactor` declares the sampling rate of the simulation results. For example, decimationFactor=1 means the results of every simulation step of the simulator are logged. And decimationFactor=10 means every 10th of the simulation results are logged. |
| <*variable*>        | `name` | Declares the name of the simulator variable to be logged.

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

The simulators with variables to be logged must be added as child elements to the `<simulators>` root element. 
The variables must be specified by its variable name in separate `<variable>` elements under the respective `<simulator>`. 
A `<simulator>` element with no `<variable>` child elements will lead to all variables being logged.
As aforementioned this could results in a large amount of data to be saved. 
It helps to reduce the logged data by the optional attribute `decimationFactor` if not every sample data is required. 
Leaving out a simulator from the configuration will disable logging for that simulator. 
The result logs are persisted as csv files.