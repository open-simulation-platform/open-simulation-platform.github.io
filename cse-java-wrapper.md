---
layout: default
title: "Java Wrapper"
permalink: /cse-java-wrapper
nav_order: 4
---

## CSE Java wrapper
The Java wrapper, _cse-core4j_, enables Java applications to make use of the `cse-core` co-simulation library.
It uses the Java Native Interface (JNI) to efficiently interact with _cse-core_. 
To make the it accessible, all native dependencies for Linux and Windows have been pre-built and are bundled with the library.

A highly convenient and powerful feature found in _cse-core4j_
is the ability to write inline FMI 2.0 compatible models
using Java. That is, models can be written as regular Java
code that can be added directly to the simulation without first
exporting them as FMUs. However, if required---for instance
in order to run the same model in another FMI/SSP compliant
tool---the model can be exported as is. This is possible thanks
to deep integration with [FMI4j](https://github.com/NTNU-IHB/FMI4j), a software package for
dealing with FMUs on the JVM. 
Below listing shows the minimal required code to write FMI 2.0 compatible models in Java using FMI4j.

```java
public class JavaSlave extends Fmi2Slave {
    @ScalarVariable (causality=Fmi2Causality.output)
    private double realOutput;

    public JavaSlave(String instanceName) {
        super(instanceName);
    }

    public void doStep(double t, double dt) {
        realOutput= ...
    }
}
```
