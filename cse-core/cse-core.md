---
layout: default
title: "CSE"
permalink: /cse-core/cse
---

## cse-core

### cse concepts

- Slaves
- Functions
- Manipulator
- Observer
- Modifiers

### Configuration

- Document OspSystemStructure.xml with link to .xsd
- Document OspModelDescription.xml with link to .xsd

### Scenario

- Document scenario file format

### Results logging

- Document results format
- Document logging configuration format

### Distributed co-simulation
Distributed simulation is currently enabled through integration with the software FMU Proxy.
- using fmu-proxy with CSE
- link to FMU-proxy doc

FMU-proxy is a framework for accessing FMUs compatible with FMI for Co-simulation and Model Exchange 2.0 in a language and platform independent way. This is achieved using well established RPC technologies. Due to the technologies involved, clients and servers for FMU-proxy can be written in almost any language, on any platform!

Server implementations already exists for C++ and JVM, while client implementations exists for C++, JVM, Python and (browser) JavaScript. And its easy to add additional implementations, as the RPC frameworks will generate most of the code for you!

FMU-proxy is different from other framework for distributed FMU invocations such as DACCOSIM, FMI GO! and Coral in that it completely separates itself from the master algorithm (logically and physically). FMU-proxy is a completely standalone project which provides access to FMUs over the wire. And just that.

The idea is that other applications should use FMU-proxy whenever FMUs are required to run distributed, rather than having each application creating their own solution.

[more details](./fmuproxy)

