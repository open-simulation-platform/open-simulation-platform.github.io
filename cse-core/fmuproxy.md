---
layout: default
title: "FMU Proxy"
permalink: /cse-core/fmuproxy
parent: "CSE Core"
---

## FMUProxy


### 1. Proxy-server

A proxy-server is responsible for making available one or more FMUs over a set of RPCs. Implementations should support Thrift and or gRPC. Additional RPCs, such as JSON-RPC can also be supported.

In addition to the RPC support, a full implementation must be able to communicate with the discovery service over HTTP. Upon starting the server, the address of a discovery service should be specified. In order to ensure that the list of available FMUs is up to date, the server must ping the discovery service over HTTP, signaling that it is still online. When enough time has passed without such a notification, the server is considered offline and is removed from the discovery service.

The framework supports both ME and CS FMUs running on the back-end, but the user is only provided with a CS API, as ME models are wrapped. The user can configure which solver will be used for wrapping the ME model, subject to availability of certain solvers, which depends on the server implementation.

Two server implementations have been realized, each described
more in detail below. Which one to deploy in production
depends on the users need for RPCs supported,
stability, stability, quality of the available ME solvers,
memory foot-print and performance. No one implementation
will excel at everything.

#### JVM

The JVM implementation is written in Kotlin and rely on FMI4j, an FMI implementation for JVM languages that supports FMI 1.0 and 2.0 for CS and ME. Out of the box, ME models can be wrapped as CS ones using solvers from the Apache Commons Math3 package. Compared to other open-source FMI implementations targeting the JVM, such as JFMI and JavaFMI, FMI4j is the only one to support ME for 2.0. Furthermore, FMI4j uses the Java Native Interface (JNI) rather than Java Native Access (JNA) for interfacing with the native FMI functions, which significantly improves performance. The calling overhead for a single native call using JNA can be an order of magnitude greater than equivalent JNI.

The implementation supports Thrift (TCP/IP - binary, HTTP - JSON), gRPC (HTTP2 - protocol buffers) and is considered as the reference implementation.

#### C++

The C++ implementation is cross-platform and is written in modern C++17. All dependencies are available using the cross-platform package manager conan, making it easy to build. Currently, Thrift (TCP/IP - binary, HTTP - JSON) and gRPC (HTTP2 - protocol buffers) are supported RPCs.

FMI4cpp, an FMI 2.0 implementation for C++, is used for interacting with FMUs. It supports both CS and ME, where the latter can be wrapped as the former using solvers from Boost odeint. The main goal of the FMI4cpp library is to be as easy to use and install as possible. To achieve this, it makes use of modern C++ features and supports installation using the vcpkg and conan package managers.

### 2. Proxy-clients

Proxy clients are used to connect with the FMUs hosted by the remote server(s). FMU-proxy aims to provide flexibility, such that clients can be implemented in a wide variety of languages and platform. 
Using Thrift or gRPC, the process of generating the required source-code for interacting with an remote FMU is quite straightforward. Listing. 1 shows the command required for generating the required sources when targeting Thrift in JavaScript. Similarly, Listing. 2 shows how C++ sources for gRPC are generated.

**Listing 1. Generating JavaScript sources for interfacing ith remote FMUs using Thrift.**

```java
thrift -js service.thift
```

**Listing 2. Generating C++ sources for interfacing with remote FMUs using gRPC.**

```java
protoc -I=. --plugin=protoc-gen-grpc= grpc_cpp_plugin --cpp_out=. -- grpc_out=. service.proto
```

FMU-proxy comes bundled with client implementations
for C++, the JVM, Python and JavaScript. The two latter
are crude and ought to be considered as proof of concept.
They are, however, bundled with the source code to
showcase how easy it is to interface with FMU-proxy from
new languages. A MATLAB demo using JSON-RPC over
HTTP is also available.

The C++ and JVM implementations are more elaborate,
providing a unified, higher level API for the users.
No matter which RPC is used, there is no difference between
a remote and local FMU slave for the user. As illustrated
by Figure. 3, they all share the same interface,
defined by FMI4cpp and FMI4j for C++ and JVM implementations
respectively. Assuming a tool is using one of
these FMI implementations, support for distributed execution
can be seamlessly added with minimal changes to the
existing code base. See Listing. 3 for an example.

<figure>
<img src="/assets/img/fmuproxyFig3.png" > 
<figcaption>Fig.3 FMI4cpp and FMI4j’s slave interface could hide
                 slaves stemming from either an in-memory implementation or
                 an actual FMU. A slave in any language supported by the chosen
                 RPC could also be implemented directly behind the RPC
                 layer. </figcaption>
</figure>

**Listing 3. JVM Thrift example, written in Kotlin.**

```kotlin
val localModel: Model = Fmu.from (<url or file>) //FMI4j API
val remoteModel: Model = ThriftFmuClient.
    socketClient(<host>, <port>). load(<guid, url or file>)
     
val model = ... // pne of the above

val stepSize = ---
val slave = model.newInstance()
slave.simpleSetup()
slave.doStep(stepSize)
slave.terminate()
```

After running the JavaScript code generation using the command shown earlier in Listing. 1, the code shown in Listing. 4 can be written. Here, Thrift is configured to use HTTP transport and JSON encoding. Subsequently an FMU slave is instantiated on the remote server and stepped for 1s until termination. The process is similar for the 14+ other languages supported by Thrift, as well as gRPC and its many supported languages.

**Listing 4. Invoking an FMU from JavaScript using Thrift over HTTP.**

```java
var transport = new Thrift.TXHRTransport("http://localhost:9091/thrift")
var protocol = new Thrift.TJSONProtocol(transport)
var client = new FmuServiceClient(protocol)
var fmu_id = client.loadFromXXX() //load from url or guid
var slave_id = client.creatInstanceFromCS(fmu_id)

client.setupExperiment(slave_id)
client.enterInitializationMode(slave_id)
client.exitInitializationMode(slave_id)

var stop =1.0
var step_size = 1.0/100
do {
    var result = client.step(slave_id, step_size)
    if (result.status !=0) {
        break
    }
} while (result.simulationTime <=stop)
client.terminate(slave_id)
```

### 3. Install dependencies
#### Conan:
Just add a new remote and re-run conan install with 
```
-o fmuproxy=True
```

conan remote add 
```
helmesjo "https://api.bintray.com/conan/helmesjo/public-conan"
```

#### Manual building:
thrift can be built manually following these [steps](https://thrift.apache.org/lib/cpp)

Download a FMU-proxy server:
[FMU-proxy](https://github.com/NTNU-IHB/FMU-proxy/releases/tag/v0.5.2) 



FMU-proxy server comes in two flavors: C++ and JVM.

Prefer to use the JVM version as it is cross-platform and has more features. Like support for FMI 1.0.

### 4. Run the FMU-proxy server:

To get started, start the server executable fmu-proxy.jar from a command line or use the bundled startup script, where -thrift/tcp 9090 tells fmu-proxy to start a Thrift RPC server listening to port 9090.
Start as many as necesssary servers on the same PC, but remember to use unique port numbers for each one. Please check that this port matches the
one(s) used in the configuration file.

#### C++:

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

#### JVM:

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


### 5. Run the fmuproxy_test located in the test folder
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