---
layout: default
title: "Model Interface Validator"
permalink: /model-interface-validator
nav_order: 7
---

## Model interface validator
The Model interface validator is a tool to verify that the simulation model complies to MSMI. 
The validator loads the OSPModelDescription.xml and the ontology, and checks that the configuration is correct. It can be used to verify that a given FMU with an OSPModelDescription.xml complies with MSMI. Or, it can be used to verify that the connections between two FMUs are semantically correct.

[Link to MSMI]()

Multi-variable connections: One of the main benefits of MSMI is that it enables the implementation of multi-variable connections with simultaneous verification of semantical correctness. 
Today connection of two FMUs is a matter of connecting signal by signal,  as shown in Fig. 1, interpreting the signal names and deducing which connections are probably correct. 
Also, ensuring matching units might be a matter of consulting documentation, or possibly communication with the model developers. 

<figure>
<img src="/assets/img/validatorFig1.png" width="300"> 
<figcaption>Fig.1 FMU variable connections today </figcaption>
</figure>

With MSMI it is possible to connect groups of signals simultaneously by grouping signals representing the same real-world phenomena into one connector, as shown in Fig. 2. Furthermore, it is possible to implement automatic checking of semantic correctness. Which could be used to guide the person performing the connections, possibly by disabling invalid connections as shown in Fig. 3.
The unit part of the ontology is developed such that it is possible to check if two signals are of the same unit. If they are, they can be connected, if they are not it is possible to check if they can be converted to the same unit and then connected. If they can be converted, the conversion factors can be retrieved from the OSPModelDescription.xml and a conversion can be inserted. 

<figure>
<img src="/assets/img/validatorFig2.png" width="300"> 
<figcaption>Fig.2 FMU multivariable connections</figcaption>
</figure>

<figure>
<img src="/assets/img/validatorFig3.png" width="300"> 
<figcaption>Fig.3 FMU multivariable connections with semantic correctness checks – Example one valid connection can be made.</figcaption>
</figure>