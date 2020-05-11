---
layout: default
title: "CSE Core"
permalink: /cse-core/cse
has_children: true
has_toc: false
nav_order: 1
---

# CSE Core
The C/C++ co-simulation library orchestrates the co-simulation of models that conform to the Functional Mock-up Interface (FMI) . 
The co-simulation library includes a fixed-step master algorithm with a configurable base step size. It allows for individually defined time steps for the models, with the requirement of being a multiple of the master algorithm base step size. During simulation, there are features to observe and manipulate simulation variables either through dedicated interfaces or with a scenario runner. It is possible to apply arbitrary operations to variable values without modifying the subsystem models themselves. 
Examples include arithmetics, logical operations, transformations, unit conversions and more. This is necessity if variables connected between models must be summed or transformed by linear transformation or between coordinate frames. There are interfaces for client applications to include their own implementation of many of the included features. 

[C/C++ co-simulation library](./doxygen/0.6.0){:target="_blank"}.

## CSE concepts
- Slaves
- Functions
- Manipulator
- Observer
- Modifiers

![foo](/assets/img/cseFig2.png "Figure 2")

## Configuration

The configuration format is based on the [MSMI specification]() and is used to configure the simulation system structure, including
connections between FMUs and setting of initial values for input and parameter variables. The configuration format is XML according to schema
[OspSystemStructure.xsd](https://github.com/open-simulation-platform/cse-core/blob/master/test/data/msmi/schema/OspModelDescription.xsd). 
As defined in the MSMI specification, `connections` are configured through variables and variableGroups. It is recommended to follow the specification for its interfaces when exporting a model for connections.
`Functions` are provided to handle manipulations of variables outside individual FMUs, for example, arithmetical operation. 
This is especially important when different models are provided by different vendors.

### _`<OspSystemStructure>` elements:_
`<OspSystemStructure>` is the root element that contains xml elements as specified below. 

| `<OspSystemStructure>`  | Description                                                                                                                               |
| :---------------------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| <*StartTime*>             | Simulation starting time, unit in seconds|
| <*BaseStepSize*>          | Base step size of co-simulation, aka, macro time step size, unit in seconds|
| <*Algorithm*>             | Co-simulation master algorithm, currently a `fixedStep` algorithm is supported|
| <*Simulators*>            | Contains all sub-simulators in the system specified as `<simulator>` elements |
| <*Functions*>             | Contains all functions, currently supported functions include  `LinearTransformation`, `Sum`, `VectorSum`                            |
| <*Connections*>          | Contains all scalar and variableGroup connections between simulators, or between simulators and functions: `<VariableConnection>`, `<SignalConnection>`, `<VariableGroupConnection>` and `<SignalGroupConnection>`                                    |

### _`<Simulator>` attributes:_ 

| attributes       | Description                                                                                                                              |
| :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| *name*           | Define an unique name for each simulator  |
| *source*         | Source of the fmu|
| *stepSize*        | Simulation step size for this individual simulator, aka. micro time step size|
| <*InitialValues*>  | Sub-element <*InitialValues*> is optional when initial values of a variable in the simulator needs to be defined. `<InitialValue>` contains attribute `variable` which is a string that refers to a FMU variable. `<InitialValue>` contains sub-element specifying the variable type `<Real>`, `<Integer>`, `<Boolean>`, or `<String>`, with attribute `value` specifying the initial value of the variable.|

### _`<Function>` attributes:_

| Function      |    attribute                       |Description                                                                                                                              |
| :--------------- | :----------------------------------| :----------------------------------------------------------------------------------------------------- |
| *LinearTransformation*           | `offset`; `factor`|  The linear transformation function is the operation that preserves the operations of addition and scalar multiplication, with attribute `offset` being the additional part and `factor` being the multiplication factor|
| *Sum*         |`inputCount` | Total count of the inputs, must match with total number of connections for this function|
| *VectorSum*         | `inputCount`; `numericType`; `dimension`| Total count of the inputs, must match with total number of connections for this function;  Numeric type can be either real or integer;  Dimension of the vector |

### _`<Connection>` attributes:_

| Connection      |    attributes                    |Description                                                                                                                              |
| :--------------- | :----------------------------------| :----------------------------------------------------------------------------------------------------- |
| <*VariableConnection*>          | `simulator`; `name`|  Contains sub-elements `<Variable>` with attributes specifying the `simulator` of which the variable belongs to, and the `name` of the variable|
| <*SignalConnection*>        |`simulator`; `function`; `name`| Contains sub-elements `<Variable>` and/or `<Signal>` with attributes specifying the `simulator` and/or `function` of which the signal belongs to, and the `name` of the signal |
| <*VariableGroupConnection*>         | `simulator`; `name`| Contains sub-elements `<VariableGroup>` with attributes specifying the `simulator` of which the variable group belongs to, and the `name` of the variable group|
| <*SignalGroupConnection*>         | `simulator`;`function`; `name`| Contains sub-elements `<VariableGroup>` and/or `<SignalGroup>` with attributes specifying the `simulator` and/or `function` of which the signal group belongs to, and the `name` of the signal croup |

An example of the code implementation is shown as follows. 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<OspSystemStructure
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://opensimulationplatform.com/MSMI/OSPSystemStructure ../../../OspSystemStructure.xsd"
        xmlns="http://opensimulationplatform.com/MSMI/OSPSystemStructure"
        version="0.1">
    <StartTime>startingTimeInSeconds</StartTime>
    <BaseStepSize>baseStepSizeInSeconds</BaseStepSize>
    <Algorithm>fixedStep</Algorithm>
    <Simulators>
        <Simulator name="model1" source="fmu1.fmu" stepSize="stepSizeInSeconds">
            <InitialValues>
                <InitialValue variable="variable1">
                    <Real value="variable1Value"/>
                </InitialValue>
                <InitialValue variable="variabl2">
                    <Boolean value="true(orFalse)"/>
                </InitialValue>
            </InitialValues>
        </Simulator>
        <Simulator name="model2" source="fmu2.fmu" stepSize="stepSizeInSeconds"/>
        ...
    </Simulators>
    <Functions>
        <LinearTransformation name="function1" factor="function1Factor" offset="function1Offset"/>
        <Sum name="function2" inputCount="function2InputCount"/>
        <VectorSum name="function2" inputCount="function2InputCount" numericType="real(orInteger)" dimension="VectorDimension"/>
    </Functions>
    <Connections>
        <VariableConnection>
            <Variable simulator="simulator1" name="variable1"/>
            <Variable simulator="simulator2" name="variable1"/>
        </VariableConnection>

        <SignalConnection>
            <Variable simulator="simulator1" name="variable1"/>
            <Signal function="function1" name="signal1"/>
        </SignalConnection>
     
        <VariableGroupConnection>
            <VariableGroup simulator="simulator1" name="variableGroup1"/>
            <VariableGroup simulator="simulator2" name="variableGroup1"/>
        </VariableGroupConnection>

        <SignalGroupConnection>
            <VariableGroup simulator="simulator1" name="variableGroup1"/>
            <SignalGroup function="function1" name="signalGroup1"/>
        </SignalGroupConnection>
    </Connections>
</OspSystemStructure>
```


As one of the standardization projects of the FMI, co-simulation configuration using the [SSP standard](https://ssp-standard.org/) is also supported. 
The normative XML Schema 1.0 schema for the MAP SSP can be found [here](https://github.com/open-simulation-platform/cse-core/tree/master/test/data/ssp/SSP10).
In cse environment, “OspSystemStructure.xml” is recommended and prioritized when both the .xml and .ssd files are available. 


## Scenario

A scenario file defines the simulation scenario which shall be performed. This serves for dynamic systems where actions to be made to trigger any event, activate models or sending signals to the controllers, etc. The example below shows the syntax structure of a scenario file. First is the `"description"` of what does this scenario do. 
`"action"` declares the action type which can be `"override"` , `"bias"`  or `"reset"`. `"events"` includes the `"time"` in second when the action takes place, on which `"model"`, the "`variable"` name in this model and the `"value"` for this variable (except for `"reset"` action type which doesn't require a value) .

```json
{ "description": "description of the scnario",
  "defaults": 
  {
    "model": "model1",
    "action": "override"
  },
  "events": 
  [
    {
      "time": event1Time,
      "variable": "variable1",
      "value": variabl1Value
    },
    {
      ...      event2       ...
    },
    {
      ...      event3       ...
    }
  ],
  "end": scenarioStopTime
}
```

## Results logging

In order to log signal values from a simulation to data files, an output directory must be specified in the result folder field on the simulation setup. By
default, all signals will be logged and persisted on every sample. There will be one file generated pr. simulator. This can quickly lead to a large
amount of data being generated, so it is recommended instead specifying what signals to log using the configurable log format outlined below.
CSE supports basic configuration of specific signals to log from any simulator via an XML file. This file must be named "LogConfig.xml" (exactly
including case) and placed in the same folder as the simulators. 

```xml
<simulators>
    <simulator name="model1" decimationFactor="20">
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

## Distributed co-simulation
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


