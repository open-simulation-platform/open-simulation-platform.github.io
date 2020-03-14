---
layout: default
title: "fmuproxy"
permalink: /cse-core/fmuproxy
---

## FMUProxy

The software architecture is shown in Fig. 1 and consists of three main parts:

<figure>
<img src="/assets/img/fmuproxyFig1.png" > 
<figcaption>Fig.1 FMU-Proxy software architecture </figcaption>
</figure>


### 1. Discovery Services

A discovery service is a web application whose main responsibility is to communicate to users information about and the location of available FMUs. This information can be obtained visually through a web interface, or programmatically through an HTTP request. 
The discovery service has the following three HTTP services:

- /availablefmus: Called by user applications. Returns a JSON formatted string containing information about all available FMUs registered with the discovery service. The information include data from the modelDescription.xml as well as the IP address of the host machine and the RPC port(s).

- /register: Called by proxy-servers on start-up. Registers the server with the discovery server. Transmits network information, and information about the modelDescription.xml for each locally available FMU.

- /ping: Called by the proxy-servers at regular   intervals, otherwise they will be considered to be offline by the discovery service.

The discovery service is an optional feature and is not required when the remote end-point of an RPC service is known to the client application, for instance when running the server on a physically accessible machine.

Multiple discovery services may be online at any given time. They may be public or used internally in a restricted network.

The discovery service has been implemented in Kotlin, a
statically typed language 100% interoperable with Java.
The front-end seen in Fig. 2 has been implemented using
PrimeFaces, a UI component framework for Java Server
Faces (JSF). It offers basic functionality such as the ability
for users to download available RPC schemas and to view
information about available FMUs in a structured way.

<figure>
<img src="/assets/img/fmuproxyFig2.png" > 
<figcaption>Fig.2 The discovery service’s web interface. </figcaption>
</figure>



### 2. Proxy-server

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

The implementation supports Thrift (TCP/IP - binary, HTTP - JSON), gRPC (HTTP2 - protocol buffers) as well as JSON-RPC (HTTP, TCP/IP and WebSockets), and is considered as the reference implementation.

#### C++

The C++ implementation is cross-platform and is written in modern C++17. All dependencies are available using the cross-platform package manager conan, making it easy to build. Currently, Thrift (TCP/IP - binary, HTTP - JSON) and gRPC (HTTP2 - protocol buffers) are supported RPCs.

FMI4cpp, an FMI 2.0 implementation for C++, is used for interacting with FMUs. It supports both CS and ME, where the latter can be wrapped as the former using solvers from Boost odeint. The main goal of the FMI4cpp library is to be as easy to use and install as possible. To achieve this, it makes use of modern C++ features and supports installation using the vcpkg and conan package managers.

### 3. Proxy-clients

Proxy clients are used to connect with the FMUs hosted by the remote server(s). FMU-proxy aims to provide flexibility, such that clients can be implemented in a wide variety of languages and platform. 
Using Thrift or gRPC, the process of generating the required source-code for interacting with an remote FMU is quite straightforward. Listing. 1 shows the command required for generating the required sources when targeting Thrift in JavaScript. Similarly, Listing. 2 shows how C++ sources for gRPC are generated.

**Listing 1. Generating JavaScript sources for interfacing ith remote FMUs using Thrift.**

`thrift -js service.thift`

**Listing 2. Generating C++ sources for interfacing with remote FMUs using gRPC.**

`protoc -I=. --plugin=protoc-gen-grpc= grpc_cpp_plugin --cpp_out=. -- grpc_out=. service.proto`

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
<figure>
<img src="/assets/img/fmuproxyFig4.png" width="500"> 
</figure>

After running the JavaScript code generation using the command shown earlier in Listing. 1, the code shown in Listing. 4 can be written. Here, Thrift is configured to use HTTP transport and JSON encoding. Subsequently an FMU slave is instantiated on the remote server and stepped for 1s until termination. The process is similar for the 14+ other languages supported by Thrift, as well as gRPC and its many supported languages.

**Listing 4. Invoking an FMU from JavaScript using Thrift over HTTP.**
<figure>
<img src="/assets/img/fmuproxyFig5.png" width="500"> 
</figure>
