---
layout: default
title: "Distributed co-simulation"
permalink: /cse-core/fmuproxy
has_toc: false
parent: "libcosim"
nav_order: 4
---
# Distributed co-simulation
`libcosim` can run distributed co-simulation which allows you to:

1. Run multiple instances of a model even though the FMU only allows it to be instantiated once.
2. Run FMUs that are not compatible with your OS or bitness.
3. Parallelize the workload onto multiple computation nodes.

Distributed simulation is currently enabled through integration with the software FMU-proxy. 
Both FMI 1.0 and FMI 2.0 for Co-simulation is supported.

The [Demo Cases](/demo-cases) include configuration for distributed co-simulation with FMU-Proxy, and this 
[section](/cse-demo-app/user-guide#distributed-co-simulation-using-fmu-proxy) shows how it's done with the CSE Demo App.

## FMU-proxy
[FMU-proxy](https://github.com/NTNU-IHB/FMU-proxy) is an open-source framework 
that enables language and platform independent access to FMUs. 
In short, FMU-proxy provides remote procedure call (RPC) mapping to the FMI 2.0 
for co-simulation interface. This is achieved by wrapping one or more FMU in a 
server program supporting multiple schema-based and language-independent RPC systems
over several network protocols. The use of schema-based RPCs allows users to easily 
auto-generate client/server code for a wide range of common programming languages. 
The framework is independent of the master algorithm, and can therefore be re-used
in different software projects. 
 
## Download and run the FMU-proxy server:

Download an FMU-proxy server from [here](https://github.com/NTNU-IHB/FMU-proxy/releases/tag/v0.6.1).

To get started, start the server executable `fmu-proxy.jar` from the command line. <br>
To see all available commands type:
 
 ```bash
java -jar fmu-proxy.jar -h
 ```

In the context of `libcosim`, only the Thrift RPC option is supported.
 
 
The command `java -jar fmu-proxy.jar -thrift/tcp 9090`
tells fmu-proxy to start a Thrift RPC server listening to port 9090.
Start as many fmu-proxy servers as necessary on the same PC, but remember to use unique port numbers. 
Please check that this port matches the source(s) specified in the configuration file.

FMUs can be pre-loaded by appending their paths at the end of the command, no switch needed.

See below for three different ways to load an FMU using fmu-proxy.

##### OspSystemStructure.xml

```xml
<Simulators>
    <Simulator name="FMU1"
        source="fmu-proxy://localhost:9090?file=path/to/fmu1.fmu"/>
    <Simulator name="FMU2"
        source="fmu-proxy://localhost:9090?url=http://example.com/fmu2.fmu"/>
    <Simulator name="FMU3"
        source="fmu-proxy://localhost:9090?guid=<fmu-guid-from-modelDescription-goes-
here>"/>
</Simulators>
```

##### SystemStructure.ssp

```xml
<ssd:Elements>
    <ssd:Component name="FMU1"
        source="fmu-proxy://localhost:9090?file=path/to/fmu1.fmu"/>
    <ssd:Component name="FMU2"
        source="fmu-proxy://localhost:9090?url=http://example.com/fmu2.fmu"/>
    <ssd:Component name="FMU3"
        source="fmu-proxy://localhost:9090?guid=<fmu-guid-from-modelDescription-goes-
here>"/>
</ssd:Elements>
```
