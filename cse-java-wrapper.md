---
layout: default
title: "cse-java-wrapper"
permalink: /cse-java-wrapper
---

## CSE Java wrapper
The Java wrapper, cse-core4j, enables Java applications to make use of the co-simulation library.
A highly convenient and powerful feature found in cse-core4j
is the ability to write inline FMI 2.0 compatible models
using Java. That is, models can be written as regular Java
code that can be added directly to the simulation without first
exporting them as FMUs. However, if required—for instance
in order to run the same model in another FMI/SSP compliant
tool—the model can be exported as is. This is possible thanks
to deep integration with FMI4j, a software package for
dealing with FMUs on the JVM. FMI4j is an open-source cross-platform Java framework for
importing and exporting FMUs. Below listing shows the minimal required code to write FMI 2.0 compatible models in Java using FMI4j.

```java
public class Java Slave extends Fmi2Slave {
    @Scalar Variable (causality=Fmi2Causality.output)
    private double realOutput;

    public javaSlave(String instanceName){
        super(instanceName);
    }

    public void doStep(double t, double dt) {
        realOutput= ...
    }
}
```