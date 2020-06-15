---
layout: default
title: "OSP validator"
permalink: /osp-validator
has_toc: false
nav_order: 6
---
<div style="text-align: right">
    <b>
        <a href="https://github.com/open-simulation-platform/osp-validator">osp-validator</a>   
        on GitHub
    </b>
</div>

# OSP validator
The [osp-validator](https://github.com/open-simulation-platform/osp-validator){:target="_blank"}
repository implements a java library and 
command line interface tool which validates an FMU with accompanying `OspModelDescription.xml` file, according to the 
[OSP-IS](https://opensimulationplatform.com/specification/){:target="_blank"} specification. 
It also validates a simulation configuration as specified by a libcosim 
`OspSystemStructure.xml` file defined [here](./libcosim/configuration.md). The documentation for the xml schemas of both
`OspModelDescription.xsd` and `OspSystemStructure.xsd` can be found [here](https://opensimulationplatform.com/xsd/){:target="_blank"}

## Java library
The jar file `osp-validator-api-<version>.jar` is found in every release of `osp-validator`, downloadable from 
[here](https://github.com/open-simulation-platform/osp-validator/releases){:target="_blank"}, and contains the java API.

The java library can be used by simulation configuration tools which needs to validate that the models and configuration
is according to OSP-IS. The [kopl](./kopl.md) configuration tool uses this library for validation.

Examples showing how it can be used in a java application can be found 
[here](https://github.com/open-simulation-platform/osp-validator/tree/0.11.0/osp-validator-api/src/main/java/com/opensimulationplatform/api/examples){:target="_blank"}

## Command line interface
A command line tool to interface the validator called `osp-cli.jar` is produced when building the `osp-validator` 
project, and it is downloadable from [here](https://github.com/open-simulation-platform/osp-validator/releases){:target="_blank"}. The 
command line interface tool can be used to verify that a given `OspModelDescription.xml` and FMU is according to the 
OSP-IS. It can also validate simulation configurations defined by a `OspSystemStructure.xml` file. If there are no 
validation errors, the output is empty. In case there are validation errors, it will print error messages for all the 
violated rules.

Calling the `osp-cli.jar` without any input arguments prints out usage help: 
```
$ java -jar osp-cli.jar
Usage: osp-cli.jar [options] [command] [command options]
  Options:
    --help, -h
      Print help
    --version, -v
      Print version
  Commands:
    osp-system-structure      Validate OspSystemStructure.xml
      Usage: osp-system-structure [options]
        Options:
          -file
            Path to OspSystemStructure.xml

    osp-model-description      Validate OspModelDescription.xml
      Usage: osp-model-description [options]
        Options:
          -file
            Path to OspModelDescription.xml
          -fmu
            Path to fmu
```

### Usage example
The .xml files shown below, which represents a valid configuration, can be validated with `osp-cli.jar` as follows:

```
$ java -jar osp-cli.jar osp-system-structure -file OspSystemStructure.xml
```

This produces no output, meaning all models, and the configuration itself is valid according to 
OSP-IS. If we change the line `<VariableGroup simulator="crane_controller" name="linear_mechanical_port"/>` in 
`OspSystemStructure.xml` to `<VariableGroup simulator="crane_controller" name="force"/>` and run the validation again, 
we see that the validator prints out an error message associated with the violated rules, and accompanying line 
number for where the error is:

```
$ java -jar osp-cli.jar osp-system-structure -file OspSystemStructure.xml
Validation error in OspSystemStructure.xml on line 11: 
VariableGroupConnection [crane_controller.force, knuckle_boom_crane.linear_mechanical_port] is invalid 
because they have different types [Force, LinearMechanicalPort]
```

### Example XML files
`CraneController_OspModelDescription.xml`
```xml
<?xml version="1.0" encoding="utf-8" ?>
<OspModelDescription xmlns="https://open-simulation-platform.com/OspModelDescription/1.0.0" version="1.0">
  <VariableGroups>
    <Generic name="actuator_limits">
      <Variable ref="Act_Limits[1]"/>
      <Variable ref="Act_Limits[2]"/>
      <Variable ref="Act_Limits[3]"/>
    </Generic>
    <LinearMechanicalPort name="linear_mechanical_port">
      <Force name="force">
        <Variable ref="p_Crane.e[1]"/>
        <Variable ref="p_Crane.e[2]"/>
        <Variable ref="p_Crane.e[3]"/>
      </Force>
      <LinearVelocity name="linear_velocity">
        <Variable ref="p_Crane.f[1]"/>
        <Variable ref="p_Crane.f[2]"/>
        <Variable ref="p_Crane.f[3]"/>
      </LinearVelocity>
    </LinearMechanicalPort>
  </VariableGroups>
</OspModelDescription>
```

`KnuckleBoomCrane_OspModelDescription.xml`
```xml
<?xml version="1.0" encoding="utf-8" ?>
<OspModelDescription xmlns="https://open-simulation-platform.com/OspModelDescription/1.0.0" version="1.0">
  <VariableGroups>
    <Generic name="actuator_limits">
      <Variable ref="Act_Limits[1]"/>
      <Variable ref="Act_Limits[2]"/>
      <Variable ref="Act_Limits[3]"/>
    </Generic>
    <LinearMechanicalPort name="linear_mechanical_port">
      <Force name="force">
        <Variable ref="p_Crane.e[1]"/>
        <Variable ref="p_Crane.e[2]"/>
        <Variable ref="p_Crane.e[3]"/>
      </Force>
      <LinearVelocity name="linear_velocity">
        <Variable ref="p_Crane.f[1]"/>
        <Variable ref="p_Crane.f[2]"/>
        <Variable ref="p_Crane.f[3]"/>
      </LinearVelocity>
    </LinearMechanicalPort>
  </VariableGroups>
</OspModelDescription>
```

`OspSystemStructure.xml`
```xml
<OspSystemStructure version="0.1" xmlns="http://opensimulationplatform.com/MSMI/OSPSystemStructure">
  <Simulators>
    <Simulator name="crane_controller" source="CraneController.fmu" stepSize="1.051732E7"/>
    <Simulator name="knuckle_boom_crane" source="KnuckleBoomCrane.fmu" stepSize="1.051732E7"/>
  </Simulators>
  <Connections>
    <VariableConnection>
      <Variable simulator="crane_controller" name="Act_Limits[1]"/>
      <Variable simulator="knuckle_boom_crane" name="Act_Limits[1]"/>
    </VariableConnection>
    <VariableGroupConnection>
      <VariableGroup simulator="crane_controller" name="linear_mechanical_port"/>
      <VariableGroup simulator="knuckle_boom_crane" name="linear_mechanical_port"/>
    </VariableGroupConnection>
  </Connections>
</OspSystemStructure>
```