---
layout: default
title: "cse-cli"
permalink: /cse-cli
---

## CSE CLI

CSE CLI is a command-line interface (CLI) to the Core Simulation Environment (CSE). It has three primary use cases:

- Running simulations from other programs or scripts
- FMU testing and debugging
- Users who simply prefer to work from the command line

Specifically, the CSE CLI can be used to perform the following tasks:

- Run a simulation based on a system structure given in either OSP XML or SSP format
- Run a simulation based on a single FMU (usually for testing/debugging purposes)
- Show information about an FMU

The output from the simulations is in the form of CSV files that can be easily parsed by other programs, for example Microsoft Excel.

With the command-line interface, the co-simulation library is used to enable co-simulation from the command-line.
