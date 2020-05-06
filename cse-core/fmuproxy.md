---
layout: default
title: "FMU Proxy"
permalink: /cse-core/fmuproxy
has_toc: false
parent: "CSE Core"
---


##  Install dependencies
### Conan:
Run conan install with the additional option:
```
-o fmuproxy=True
```

### Manual building:
Thrift can be built manually following these [steps](https://thrift.apache.org/lib/cpp).



## Download and run the FMU-proxy server:

Download an FMU-proxy server from [here](https://github.com/NTNU-IHB/FMU-proxy/releases/tag/v0.6.1).

To get started, start the server executable `fmu-proxy.jar` from the command line. <br>
To see all available commands type:
 
 ```bash
java -jar fmu-proxy.jar -h
 ```

In the context of CSE, only the Thrift RPC option is supported.
 
 
The command `java -jar fmu-proxy.jar -thrift/tcp 9090`
tells fmu-proxy to start a Thrift RPC server listening to port 9090.
Start as many fmu-proxy servers as necessary on the same PC, but remember to use unique port numbers for each one. 
Please check that this port matches the source(s) specified in the configuration file.

```bash
start java -jar fmu-proxy.jar -thrift/tcp 9090
```

FMUs can be pre-loaded by appending their paths at the end of the command, no switch needed.

See below for three different ways to load an FMU using fmu-proxy.

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

[More details of FMU-proxy](https://github.com/NTNU-IHB/FMU-proxy). 
