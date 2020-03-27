---
layout: default
title: "cse-cli"
permalink: /cse-cli
---

## CSE CLI
CSE CLI is a command-line interface (CLI) to the Core Simulation Environment (CSE). It facilitates for running co-simulations of systems configured with SSP or MSMI. 
It has three primary use cases:

- Running simulations from other programs or scripts
- FMU testing and debugging
- Users who simply prefer to work from the command line

Specifically, the CSE CLI can be used to perform the following tasks:

- Run a simulation based on a system structure given in either OSP XML or SSP format
- Run a simulation based on a single FMU (usually for testing/debugging purposes)
- Show information about an FMU

The command line application is suitable when multiple sets of simulations can be executed in parallel. An example for such use case could be in design, where there typically are multiple system variants and parametrizations of the system.
It can simulate single FMUs and show the model descriptions. Besides being a necessary simulation tool, the CSE command line interface also serves as a demonstration for how client applications can make use of the CSE co-simulation library.
With the command-line interface, the co-simulation library is used to enable co-simulation from the command-line. 
