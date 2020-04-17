---
layout: default
title: "cse-cli"
permalink: /cse-cli
---

## CSE CLI
For the cases where a graphical user interface is not available or needed, the CSE comes as a command line application. CSE-CLI is a command-line interface (CLI) to CSE. It facilitates for running co-simulations of systems configured with SSP or MSMI. 
It has three primary use cases:

- Running simulations from other programs or scripts
- FMU testing and debugging
- Users who simply prefer to work from the command line

The command line application is suitable when multiple sets of simulations can be executed in parallel. An example for such use case could be in design, where there typically are multiple system variants and parameterization of the system.
It can simulate single FMUs and show the model descriptions. Besides being a necessary simulation tool, the CSE command line interface also serves as a demonstration for how client applications can make use of the CSE co-simulation library.
With the command-line interface, the co-simulation library is used to enable co-simulation from the command-line. 


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


#### General usage

```shell
> cse
Error: Missing command-line arguments.  Run "cse help" to get help.
```

```shell
> cse help
NAME
  cse - Command-line interface to the Core Simulation Environment

SYNOPSIS
  cse <subcommand> <args...> [options...]

DESCRIPTION
  The Core Simulation Environment is free and open-source software for running distributed co-simulations.

SUBCOMMANDS
  help                Shows documentation
  inspect             Shows information about a model
  run                 Runs a simulation
  run-single          Runs a simulation with a single subsimulator

OPTIONS
  --help                     Display a help message and exit.
  --log-level arg (=warning) Sets the detail/severity level of diagnostic program output.  Valid argument values, in
                             order of increasing detail (and decreasing severity), are: error, warning, info, debug,
                             trace.
  -v [ --verbose ]           Shorthand for --log-level=info.
  --version                  Display program version information and exit.
 
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
  cse run <system_structure_path> [options...]
 
DESCRIPTION
  The Core Simulation Environment is free and open-source software for running distributed co-simulations.

SUBCOMMANDS
  help                Shows documentation
  inspect             Shows information about a model
  run                 Runs a simulation
  run-single          Runs a simulation with a single subsimulator

OPTIONS
  --help                     Display a help message and exit.
  --log-level arg (=warning) Sets the detail/severity level of diagnostic program output.  Valid argument values, in
                             order of increasing detail (and decreasing severity), are: error, warning, info, debug,
                             trace.
  -v [ --verbose ]           Shorthand for --log-level=info.
  --version                  Display program version information and exit.


C:\Users\yingguangc\Downloads\cse-cli-v0.2.0-win64\bin>cse help <inspect>
The syntax of the command is incorrect.

C:\Users\yingguangc\Downloads\cse-cli-v0.2.0-win64\bin>cse help run
NAME
  cse run - Runs a simulation

SYNOPSIS
  cse run <system_structure_path> [options...]

DESCRIPTION
  This command runs a simulation based on a configuration file that specifies the system structure, i.e., which models to
  include and the connections between them.

  The simulation can be synchronised with real time by using the the '--real-time' option, optionally specifying a target
  real-time factor (RTF).  The RTF is defined as 'elapsed logical time divided by elapsed real time in seconds'.  In other
  words:

  * RTF < 1 means slower than real time
  * RTF = 1 means real time
  * RTF > 1 means faster than real time

  Whether the target RTF can actually be reached depends on the simulation setup, most importantly the models being simulated,
  and the machine(s) on which the simulation is being executed.

  If '--real-time' is not specified, the default is to run as fast as possible, unconstrained by real time.

PARAMETERS
  system_structure_path       The path to the system structure definition file/directory.  If this is a file with .xml
                              extension, or a directory that contains a file named OspSystemStructure.xml, it will be
                              interpreted as a OSP system structure definition.  Otherwise, it will be interpreted as an SSP
                              system structure definition.

OPTIONS
  -b [ --begin-time ] arg (=0)     The logical time at which the simulation should start.
  -d [ --duration ] arg (=1)       The duration of the simulation, in logical time.  Excludes -e/--end-time.
  -e [ --end-time ] arg            The logical end time of the simulation.  Excludes -d/--duration.
  --help                           Display a help message and exit.
  --log-level arg (=warning)       Sets the detail/severity level of diagnostic program output.  Valid argument values, in
                                   order of increasing detail (and decreasing severity), are: error, warning, info, debug,
                                   trace.
  --mr-progress [=resolution(=10)] Enables printing of machine-readable progress indicator lines on the form '@progress n t
                                   d'.  Here, n indicates the progress in 1/N-ths of the total time, where N is the resolution
                                   given as an argument to this option.  t is the current logical time and d is the amount of
                                   logical time that has passed since the start of the simulation.  t and d are floating-point
                                   numbers while n is an integer.
  --output-config arg (=auto)      The path to an XML file that contains configuration settings for simulation output, or one
                                   of the special values 'auto', 'all' and 'none'.  The default is 'auto', which causes the
                                   program to look for a file named 'LogConfig.xml' in the same directory as the system
                                   structure definition file.  If no such file is found, the effect is the same as for 'all',
                                   meaning that the values of all variables are stored for each time step.  'none' disables
                                   file output altogether.
  --output-dir arg (=.)            The path to a directory for storing simulation results.
  --real-time [=target_rtf(=1)]    Enables real-time-synchronised simulations.  A target RTF may optionally be specified, with
                                   a default value of 1.
  --scenario arg                   The path to a scenario file to run.  By default, no scenario is run.
  --scenario-start arg (=0)        The logical time at which the scenario will start.  Only used if --scenario is specified.
  -v [ --verbose ]                 Shorthand for --log-level=info.
  --version                        Display program version information and exit.
```

#### Running a single model

```shell
> cse help run-single
NAME
    cse run-single - Runs a simulation with a single subsimulator.
 
SYNOPSIS
  cse run-single <uri_or_path> <initial_value...> [options...]

DESCRIPTION
  This command runs a simulation consisting of a single subsimulator.  This is mainly meant for testing and debugging.  The
  simulator is therefore run directly, without the overhead and extra machinery of a full co-simulation.

  The model to simulate can be specified with a URI, or, if it's a local FMU, by its path.  Using a path is equivalent to using
  a 'file' URI.

  Initial variable values may be specified after the model URI/path, by supplying a list of name=value pairs as individual
  command-line arguments.

  The simulation can be synchronised with real time by using the '--real-time' option, optionally specifying a target real-time
  factor (RTF).  The RTF is defined as 'elapsed logical time divided by elapsed real time in seconds'.  In other words:

  * RTF < 1 means slower than real time
  * RTF = 1 means real time
  * RTF > 1 means faster than real time

  Whether the target RTF can actually be reached depends on the simulation setup, most importantly the models being simulated,
  and the machine(s) on which the simulation is being executed.

  If '--real-time' is not specified, the default is to run as fast as possible, unconstrained by real time.

PARAMETERS
  uri_or_path           A model URI or FMU path.
  initial_value         Initial values for model variables, on the form <name>=<value>.  Allowed values for boolean variables
                        are 'true' or 'false'.

OPTIONS
  -b [ --begin-time ] arg (=0)            The logical time at which the simulation should start.
  -d [ --duration ] arg (=1)              The duration of the simulation, in logical time.  Excludes -e/--end-time.
  -e [ --end-time ] arg                   The logical end time of the simulation.  Excludes -d/--duration.
  --help                                  Display a help message and exit.
  --log-level arg (=warning)              Sets the detail/severity level of diagnostic program output.  Valid argument values,
                                          in order of increasing detail (and decreasing severity), are: error, warning, info,
                                          debug, trace.
  --mr-progress [=resolution(=10)]        Enables printing of machine-readable progress indicator lines on the form '@progress
                                          n t d'.  Here, n indicates the progress in 1/N-ths of the total time, where N is the
                                          resolution given as an argument to this option.  t is the current logical time and d
                                          is the amount of logical time that has passed since the start of the simulation.  t
                                          and d are floating-point numbers while n is an integer.
  --output-file arg (=./model-output.csv) The file to which simulation results should be written.
  --real-time [=target_rtf(=1)]           Enables real-time-synchronised simulations.  A target RTF may optionally be
                                          specified, with a default value of 1.
  -s [ --step-size ] arg (=0.01)          The co-simulation step size.
  -v [ --verbose ]                        Shorthand for --log-level=info.
  --version                               Display program version information and exit.
```

