---
layout: default
title: "FMU Proxy"
permalink: /cse-core/fmuproxy
parent: "CSE Core"
---


##  Install dependencies
### Conan:
Just add a new remote and re-run conan install with 
```
-o fmuproxy=True
```

conan remote add 
```
helmesjo "https://api.bintray.com/conan/helmesjo/public-conan"
```

### Manual building:
thrift can be built manually following these [steps](https://thrift.apache.org/lib/cpp)

Download a FMU-proxy server:
[FMU-proxy](https://github.com/NTNU-IHB/FMU-proxy/releases/tag/v0.5.2) 



FMU-proxy server comes in two flavors: C++ and JVM.

Prefer to use the JVM version as it is cross-platform and has more features. Like support for FMI 1.0.

## Run the FMU-proxy server:

To get started, start the server executable fmu-proxy.jar from a command line or use the bundled startup script, where -thrift/tcp 9090 tells fmu-proxy to start a Thrift RPC server listening to port 9090.
Start as many as necesssary servers on the same PC, but remember to use unique port numbers for each one. Please check that this port matches the
one(s) used in the configuration file.

### C++:

```
./fmu_proxy_cpp -thrift/tcp 9090
```

You can pre-load FMUs using the -f switch.

```
FMU-proxy
Options:
  -h [ --help ]         Print this help message and quits.
  --fmu arg             Path to FMUs.
  --thrift/tcp arg      Specify the Thrift tcp port (enables this server).
  --thrift/http arg     Specify the Thrift http port  (enables this server).
  --grpc arg            Specify gRPC port (enables this server).
```

### JVM:

```java
start java -jar fmu-proxy.jar -thrift/tcp 9090
```

FMUs can be pre-loaded by appending the paths at the end of the command, no switch needed.

For each executable -h will display a help message with available options.

```
Usage: fmu-proxy [-h] 
                 [-grpc=<grpcPort>]
                 [-r=<remote>] [-thrift/http=<thriftHttpPort>]
                 [-thrift/tcp=<thriftTcpPort>] FMUs...
      FMUs...             FMU(s) to include.
      -grpc=<grpcPort>    Specify the gRPC port (enables this server).
  -h, --help              Print this message and quits.
      -thrift/http=<thriftHttpPort> Specify the Thrift http port (enables this server).
      -thrift/tcp=<thriftTcpPort> Specify the Thrift tcp port (enables this server).
```


## Run the fmuproxy_test located in the test folder
This test takes 3 arguments:

- url
- host
- port

Download an [Example](http://folk.ntnu.no/laht/files/ControlledTemperature.fmu) localhost 9090

Note: If you pre-loaded the server executable with some FMUs, you can use from_guid rather than from_url. (The guid is the guid found in the modelDescription.xml of the requested FMU)

See below for three different ways to specify the FMU to be loaded by fmu-proxy.

```xml
<Simulators>
    <Simulator name="FMU1"
        source="fmu-proxy://localhost:9090?file=path/to/fmu1.fmu"/>
    <Simulator name="FMU2"
        source="fmu-proxy://localhost:9090?http://example.com/fmu2.fmu"/>
    <Simulator name="FMU3"
        source="fmu-proxy://localhost:9090?guid=<fmu-guid-from-modelDescriptiongoes-
here>"/>
</Simulators>
```

[More details of FMU-proxy](https://github.com/NTNU-IHB/FMU-proxy). 
