---
layout: default
title: "Simulation configuration"
permalink: /libcosim/configuration
has_toc: false
parent: "Co-simulation C/C++ libraries"
nav_order: 1
---
# Simulation configuration

The OSP system structure configuration format is based on the OSP Inteface Specification 
[OSP-IS](https://open-simulation-platform.com/specification/){:target="_blank"} 
and is used to configure the simulation system structure, including
connections between FMUs and setting of initial values for input and parameter variables. The configuration format is XML according to schema
[OspSystemStructure.xsd](https://open-simulation-platform.com/xsd/OspSystemStructure-0.1.xsd){:target="_blank"}. 
Description of the OspSystemStructure elements and their attributes are shown below. Code implementation examples follow.

## \<OspSystemStructure>
`<OspSystemStructure>` is the root element that contains xml elements as specified below. 

| `<OspSystemStructure>`  | Description                                                                                                                               |
| :---------------------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| <*StartTime*>             | Simulation starting time, unit in seconds.|
| <*BaseStepSize*>          | Base step size of co-simulation, aka, macro time step size, unit in seconds.|
| <*Algorithm*>             | Co-simulation master algorithm, currently a `fixedStep` algorithm is supported.|
| <*Simulators*>            | Contains all sub-simulators in the system specified as `<simulator>` elements.|
| <*Functions*>             | Contains all functions, currently supported functions include  `LinearTransformation`, `Sum`, `VectorSum`.                            |
| <*Connections*>          | Contains all scalar and variableGroup connections between simulators, or between simulators and functions. Sub-elements may include: `<VariableConnection>`, `<SignalConnection>`, `<VariableGroupConnection>` and `<SignalGroupConnection>`.                                   |

```xml
<?xml version="1.0" encoding="utf-8" ?>
<OspSystemStructure
        xmlns="http://opensimulationplatform.com/MSMI/OSPSystemStructure"
        version="0.1">
    <StartTime>0.0</StartTime>
    <BaseStepSize>0.01</BaseStepSize>
    <Algorithm>fixedStep</Algorithm>
    <Simulators> ... <Simulators>
    <Functions> ... </Functions>
    <Connections> ... </Connections>
</OspSystemStructure>
```

## \<Simulator> 
Each running simulator must be assigned to a model(fmu). One FMU can be used by several simulators given different names. 
`<Simulator>` may contain sub-element `<InitialValues>`. `<InitialValues>` is optional when initial values of one or multiple variables in the simulator needs to be defined.

| attribute       | Description                                                                                                                              |
| :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| `name`           | Define an unique name for each simulator.  |
| `source`         | Source of the simulator, example below shows three different options to specify the path to a simulator.|
| `stepSize`        | Simulation step size for this individual simulator, aka. micro time step size.|

| sub-element      | attribute | Description                                                                                                                              |
| :--------------- | :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| <*InitialValues*> | `variable` | Contains sub-element  <*InitialValue*> defying all initial values of variables in need. Attribute `variable` is a string that refers to a variable in the simulator.|
| <*InitialValue*> | `value`  | Contains sub-element specifying the variable type which can be `<Real>`, `<Integer>`, `<Boolean>`, or `<String>`. Attribute `value` gives the initial value of the variable.|

```xml
    <Simulators>
        <Simulator name="model1" source="../fmu1.fmu" stepSize="0.001">
            <InitialValues>
                <InitialValue variable="variable1">
                    <Real value="0.1"/>
                </InitialValue>
                <InitialValue variable="variabl2">
                    <Boolean value="true"/>
                </InitialValue>
            </InitialValues>
        </Simulator>
        <Simulator name="model2" source="fmu2.fmu" stepSize="0.001"/>
        <Simulator name="model3" source="proxyfmu://localhost?file=fmu3.fmu" stepSize="0.001"/>
        ...
    </Simulators>
```

## \<Function>
`Functions` are provided to handle manipulations of variables outside individual FMUs, for example, arithmetical operation. 
This is especially important when different models are provided by different vendors.

| Function      |    attribute                       |Description                                                                                                                              |
| :--------------- | :----------------------------------| :----------------------------------------------------------------------------------------------------- |
| *LinearTransformation*           | `offset`|  The linear transformation function is the operation that preserves the operations of addition and scalar multiplication, with attribute `offset` being the additional part.|
|        | `factor`|  `factor` is the multiplication factor of the linear transformation function.|
| *Sum*         |`inputCount` | Total count of the inputs, must match with total number in connections for this function.|
| *VectorSum*         | `inputCount`| Total count of the inputs, must match with total number in connections for this function. |
|        |  `numericType`| Numeric type can be either real or integer. |
|       | `dimension`| Dimension of the vector. |

```xml
   <Functions>
        <LinearTransformation name="function1" factor="0.1" offset="1"/>
        <Sum name="function2" inputCount="3"/>
        <VectorSum name="function2" inputCount="2" numericType="real" dimension="3"/>
    </Functions>
```

## \<Connection>
As defined in OSP-IS, `connections` are configured through variables and variableGroups. It is highly recommended to follow the specification for its interfaces when exporting a model for connections.

| sub-element     |    attribute                   |Description                                                                                                                              |
| :--------------- | :----------------------------------| :----------------------------------------------------------------------------------------------------- |
| <*VariableConnection*>          | `simulator`; `name`|  Contains sub-elements `<Variable>` with attributes specifying the `simulator` of which the variable belongs to and name of the variable.|
| <*SignalConnection*>        |`simulator`; `name`| Contains sub-elements `<Variable>` with attributes specifying the `simulator` of which the variable belongs to and `name` of the variable.|
|        |`function`; `name`| Contains sub-elements `<Signal>` with attributes specifying the `function` of which the signal belongs to and name of the signal.|
| <*VariableGroupConnection*>         | `simulator`; `name`| Contains sub-elements `<VariableGroup>` with attributes specifying the `simulator` of which the variable group belongs to, and the `name` of the variable group.|
| <*SignalGroupConnection*>         | `simulator`; `name`| Contains sub-elements `<VariableGroup>` with attributes specifying the `simulator` of which the variable group belongs to, and the `name` of the variable group. |
|       | `function`; `name`| Contains sub-elements `<SignalGroup>` with attributes specifying the `function` of which the signal group belongs to, and the `name` of the signal group. |

```xml
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
```

Alternatively, co-simulation configuration using the [SSP standard](https://ssp-standard.org/){:target="_blank"} is also supported. The SSP standard is one of the standardization projects of the FMI standard. 
The normative XML Schema 1.0 schema for the MAP SSP can be found [here](https://github.com/open-simulation-platform/cse-core/tree/master/test/data/ssp/SSP10){:target="_blank"}.
`OspSystemStructure.xml` is recommended and prioritized when both the .xml and .ssd files are available. 
