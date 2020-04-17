---
layout: default
title: "cse-cli"
permalink: /cse-cli
---

## CSE CLI
For the cases where a graphical user interface is not available or needed, the CSE comes as a command line application. CSE CLI is a command-line interface (CLI) to the Core Simulation Environment (CSE). It facilitates for running co-simulations of systems configured with SSP or MSMI. 
It has three primary use cases:

- Running simulations from other programs or scripts
- FMU testing and debugging
- Users who simply prefer to work from the command line

The command line application is suitable when multiple sets of simulations can be executed in parallel. An example for such use case could be in design, where there typically are multiple system variants and parametrizations of the system.
It can simulate single FMUs and show the model descriptions. Besides being a necessary simulation tool, the CSE command line interface also serves as a demonstration for how client applications can make use of the CSE co-simulation library.
With the command-line interface, the co-simulation library is used to enable co-simulation from the command-line. 

This page collects design ideas for the CSE command-line interface (CLI).

### Functionality and design

CSE-CLI should be able to perform the following tasks:

- Run a simulation, where the system to simulate is specified as an SSP file.
- Provide information about a model (usually an FMU).
- Do a "test run" with a single model (FMU).
The following is an unsorted list of loose design ideas and desirable features. A more detailed specification is given in the form of "help texts" in the next section.

- Ability to access both local and remote models, using paths or URIs.
- Logging of simulation results to disk.
- Git-like CLI
--Subcommands for main operations
--GNU-style long options (--option) with one-letter short forms (-o) for the most commonly used ones
--Built-in help/documentation.

### Functional specification

Here, we specify the functionality of the software in the form of imagined interactions with a user. This includes a somewhat formal specification given by the output of the cse help commands.

#### General usage

```shell
> cse
Error: Missing command-line arguments.  Run "cse help" to get help.
```

```shell
> cse help
NAME
    cse - Command line interface to the Core Simulation Environment.
 
SYNOPSIS
    cse [--version] [--help] [--log-level=<level>] <command> [<args>]
 
DESCRIPTION
    The Core Simulation Environment is free and open-source software for
    running distributed co-simulations.
 
OPTIONS
    --version
        Prints the CSE version number.
 
    --help
        Prints help on using CSE.  Include a subcommand to get detailed
        help on that command.  The following are equivalent:
            cse --help [<command>]
            cse [<command>] --help
            cse help [<command>]
 
    --log-level=<level>      (default: --log-level=warning)
        Sets the detail/severity level of diagnostic program output.
        Valid <level> values, in order of increasing detail (and decreasing
        severity) are: error, warning, info, debug, trace.
 
COMMANDS
 
    help
        Shows documentation.  Run "cse help <command>" for detailed
        information on one of the above commands.
 
    inspect
        Shows information about a model.
 
    run
        Runs a simulation.
 
    run-single
        Runs a simulation with a single subsimulator.
```

```shell
> cse --version
CSE CLI 0.2.0
```

#### Getting information about a model
```shell
> cse help inspect
NAME
    cse inspect - Shows information about a model
 
SYNOPSIS
    cse inspect <uri>
 
    cse inspect <path>
 
DESCRIPTION
    This command shows information about a model, such as its name,
    description, author, version, and so on.  It also lists information
    about the model's variables, like their names, types, causalities,
    variabilities, etc.
 
    The model can be specified with a URI, or, if it's a local FMU, by its
    path.  Using a path is equivalent to using a `file` URI.
```
```shell
> cse inspect models/relativistic_energy.fmu
name: physics.relativity.energy
author: Albert Einstein
version: 1.0
description: Calculates relativistic energy, E^2 = m^2 c^4 + p^2 c^2
variables:
  - name: mass
    type: real
    causality: parameter
    variability: tunable
  - name: momentum
    type: real
    causality: input
    variability: continuous
  - name: energy
    type: real
    causality: output
    variability: continuous
```

```shell
> cse inspect fmu-proxy://10.0.0.65:9090?url=http://simple-models.com/circle_area.fmu
name: math.geometry.area.circle
author: Archimedes
version: 3.14.159
description: Calculates the area of a circle, A = pi r^2
variables:
  - name: radius
    type: real
    causality: input
    variability: continuous
  - name: area
    type: real
    causality: output
    variability: continuous
```
#### Running a simulation

```shell
> cse help run
NAME
    cse run - Runs a simulation.
 
SYNOPSIS
    cse run <ssp file> [-a <algorithm>] [-b <number>]
        [-d <number> | -e <number>] [-s <number>] [--rtf=<number>]
        [--output-dir <directory>]
 
DESCRIPTION
    This command runs a simulation based on an SSP file that specifies
    which models to include and the connections between them.
 
OPTIONS
    -a <algorithm>, --algorithm=<algorithm>     (default: fixed-stepsize)
        Which co-simulation algorithm to use.  Available algorithms are:
        fixed-stepsize.
 
    -b <number>, --begintime=<number>       (default: 0)
        The logical time at which the simulation should start.
 
    -d <number>, --duration=<number>        (default: 1)
        The duration of the simulation, in logical time.  Excludes -e.
 
    -e <number>, --endtime=<number>
        The logical end time of the simulation. Excludes -d.
 
    --output-dir=<directory>                (default: .)
        Where to write log files with simulation results.
 
    --rtf
        The desired real-time factor. 1 means real time. The default if
        not specified is to run as fast as possible.
 
    -s <number>, --stepsize=<number>        (default: 0.01)
        The step size.
```

```shell
> cse run sea_trial.ssp -d 3600 --output-dir=sim/results --log-level=info
[info] Initialising models
[info] Starting simulation, t = 0
[info] 10% complete, t = 360
[info] 20% complete, t = 720
[info] 30% complete, t = 1080
[info] 40% complete, t = 1440
[info] 50% complete, t = 1800
[info] 60% complete, t = 2160
[info] 70% complete, t = 2520
[info] 80% complete, t = 2880
[info] 90% complete, t = 3240
[info] Simulation complete, t = 3600    
```

#### Running a single model

```shell
> cse help run-single
NAME
    cse run-single - Runs a simulation with a single subsimulator.
 
SYNOPSIS
    cse run-single (<uri> | <path>) [<variable>=<value> ...] [-b <number>]
        [-d <number> | -e <number>] [-s <number>] [--rtf=<number>]
        [--output-file=<path>]
 
DESCRIPTION
    This command runs a simulation consisting of a single subsimulator.
    This is mainly meant for testing and debugging. The simulator is
    therefore run directly, without the overhead and extra machinery of
    a full co-simulation.
 
    The model to simulate can be specified with a URI, or, if it's a local
    FMU, by its path.  Using a path is equivalent to using a `file` URI.
 
    Initial variable values may be specified after the model URI/path, by
    supplying a list of name=value pairs as individual command-line
    arguments
 
OPTIONS
    -b <number>, --begintime=<number>       (default: 0)
        The logical time at which the simulation should start.
 
    -d <number>, --duration=<number>        (default: 1)
        The duration of the simulation, in logical time.  Excludes -e.
 
    -e <number>, --endtime=<number>
        The logical end time of the simulation. Excludes -d.
 
    --output-file=<path>                    (default: model-output.csv)
        The file to which simulation results will be written.
 
    --rtf
        The desired real-time factor. 1 means real time. The default if
        not specified is to run as fast as possible.
 
    -s <number>, --stepsize=<number>        (default: 0.01)
        The step size.
```

```shell
> cse run-single powerplant.fmu -d 3600 --output-dir=sim/results
```