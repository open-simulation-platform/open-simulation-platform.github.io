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
Download latest `osp-cli.jar` from [here](https://github.com/open-simulation-platform/osp-validator/releases), and 
example `OspSystemStructure.xml`, `OspModelDescription.xml` and FMUs from 
[here](https://github.com/open-simulation-platform/osp-validator/tree/master/osp-validator-cli/src/main/resources/example). 
The FMUs only contain a `modelDescription.xml` file in order to be able to test `osp-cli.jar`. This configuration is 
valid and so the `osp-cli.jar` will produce no output when validating, as shown below.

```
$ java -jar osp-cli.jar osp-system-structure -file OspSystemStructure_Crane.xml
```

If we change line 12 in `OspSystemStructure_Crane.xml` from 
`<VariableGroup simulator="crane_controller" name="linear_mechanical_port"/>` to 
`<VariableGroup simulator="crane_controller" name="force"/>` and re-run the validator, it prints the validation error
messages to standard out, as shown below.
```
$ java -jar osp-cli.jar osp-system-structure -file OspSystemStructure_Crane.xml
Validation error in OspSystemStructure_Crane.xml on line 11: VariableGroupConnection 
[crane_controller.force, knuckle_boom_crane.linear_mechanical_port] is invalid because they have different types 
[Force, LinearMechanicalPort]
```

As we can see from the output, the `VariableGroupConnection` found on line 11 in `OspSystemStructure_Crane.xml` is 
invalid, because it tries to connect two variable groups of incompatible types (`Force` and `LinearMechanicalPort`).