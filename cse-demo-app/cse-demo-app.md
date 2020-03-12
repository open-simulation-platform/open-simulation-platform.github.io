---
layout: default
title: "cse-demo-app"
permalink: /cse-demo-app/cse-demo-app
---

## CSE demo application
A STEP BY STEP USER GUIDE ON HOW TO USE THE OSP CORE SIMULATION ENVIRONMENT (CSE) DEMO APPLICATION.

VERSION: 0.6.0

This application demonstrates the features of cse-core, and is a snapshot of the development done in OSP Work Package 1 so far. Basic
co-simulation functionality is demonstrated with a simple graphical user interface:
- load co-simulation configuration
- simulation control
- observe simulation variables
- override/set simulation variables
- running scenarios
- plotting variables
- logging results to file

The intention of this application is to provide a user interface for the cse-core. The look and feel of CSE demo will be subject to change. Features
requested by the JIP use cases will be demonstrated in following releases.

### User guide

#### Download and install 

Antivirus alert: Be aware that your virus protection software might react to the cse application. It's not harmful and you can allow it to run.

- Windows
1. Download CSE demo application: cse-v0.5.0-win64.zip
2. Extract the files and you will have a root folder looking like below

![foo](/assets/img/CSEuserguideFig1.png "Figure 1")

3. Download dp-ship example models: dp-ship.zip
4. Extract the demo models, for instance next to the cse-v0.5.0-win64 folder
5. Run startup script: run-windows.cmd
6. The application should open in your web browser at url http://localhost:8000.

-Linux
1. Download CSE demo application archive: cse-server-go-v0.5.0.tar.gz
2. Extract archive: tar -xzvf cse-server-go-v0.5.0.tar.gz
3. Download dp-ship example models: dp-ship.zip
4. Extract the demo models, for instance next to the cse-v0.5.0-win64 folder
5. Run startup script: run-linux
6. The application should open in your web browser at url http://localhost:8000.

#### Loading Configuration
1. Switch to your web browser at url http://localhost:8000.
2. Enter the path to the dp-ship folder on your machine.
3. Enter the path to a directory where you want the logs to be written. Leave it blank to disable file logging.
4. Previously used configurations are available below.
5. Click "Load simulation" to load the configuration in the FMUs path.

![foo](/assets/img/CSEuserguideFig2.png "Figure 2")

#### Models and variables
1. The models in the simulation will appear on the left hand side.
2. Browse the model variables by clicking the names. Their variables are organized in tabs based on causality.
3. The simulation starts when clicking "Play".

![foo](/assets/img/CSEuserguideFig3.png "Figure 3")

#### Simulation control
1. Click "Simulation status" to see details about the simulation and to access some simulation controls.
2. By clicking "Enable" on the real time target, the simulation will run in real time. If real time target is disabled, the simulation will run as fast
as possible.
3. RTF (Real time factor) indicates simulation speed related to real time. RTF = 2 means that simulation runs twice the speed of real time.
4. Real time factor target value can be altered to set a specific RTF.
5. Click play/pause to start or pause the simulation.
6. To close the current simulation for loading another configuration, click the "Tear down" button.

![foo](/assets/img/CSEuserguideFig4.png "Figure 4")

#### Plotting variables
1. Click "Create new time series" or "Create new XY plot" to obtain a time series plot or an XY plot that variables can be added to.
2. Any variable can be added to a plot by clicking "Add to plot" in the variable overview.
3. Select where to plot the variable from the list of available plots.
4. Click the plot names to see the charts.

![foo](/assets/img/CSEuserguideFig5.png "Figure 5")
![foo](/assets/img/CSEuserguideFig6.png "Figure 6")

#### Overriding variables

It is possible to override any input, output or parameter variable.
1. Click the edit icon to the left of the variable.
2. Type in the value and click the check icon.
3. Click the eraser symbol to remove the override.

![foo](/assets/img/CSEuserguideFig7.png "Figure 7")

#### Scenarios

Scenarios can be defined with the syntax described in the table below. [link to scenario format spec].

![foo](/assets/img/CSEuserguideFig8.png "Figure 8")

1. Scenario files placed in the ./scenarios subfolder within the configuration folder are automatically be loaded and made visible in the
Scenarios section.The dp-ship example comes with three scenario files for demonstration purposes.
2. Click the scenarios to see its contents.
3. To execute a scenario click "Load scenario" before starting the simulation.

![foo](/assets/img/CSEuserguideFig9.png "Figure 9")

#### Co-simulation configuration

The configuration format is based on the MSMI standard developed in WP2 and is used to configure the simulation system structure, including
connections between FMUs and setting of initial values for input and parameter variables. The configuration format is XML according to schema
OspSystemStructure.xsd. [Link to OspSystemStructure spec]. Connections can be configured as PlugSocketConnections, BondConnections and VariableConnections. To be able to
use the PlugSocketConnections and the BondConnections, you will also have to provide a ModelName_OspModelDescription.xml file according
to schema OspModelDescription.xsd for each of the FMUs that will use them. The dp-ship, quarter-truck and house examples each contain a
OspSystemStructure.xml file that demonstrates how this is done.

![foo](/assets/img/CSEuserguideFig10.png "Figure 10")

Co-simulation configuration using the SSP standard (https://ssp-standard.org/) is also partially supported. [Link to ssp support]. The dp-ship and house examples each
contain a SystemStructure.ssd file that demonstrates this.
CSE currently supports using a fixed step algorithm. The step size can be altered in SystemStructure.ssd:
SystemStructureDescription.System.SimulationInformation.FixedStepMaster:stepSize. The new MSMI connection types are not supported when
using the SSP standard.

#### Configurable logging
In order to log signal values from a simulation to file, an output directory must be specified in the "Log folder" field on the simulation setup. By
default, all signals will be logged and persisted on every sample. There will be one file generated pr. simulator. This can quickly lead to a large
amount of data being generated, so we recommend instead specifying what signals to log using the configurable log format outlined below. [Link to logger configuration].
CSE supports basic configuration of specific signals to log from any simulator via an XML file. This file must be named "LogConfig.xml" (exactly
including case) and placed in the same folder as the simulators. A basic example of the currently supported syntax is;

![foo](/assets/img/CSEuserguideFig11.png "Figure 11")

The simulators to be logged must be enclosed in a <simulators> tag, and each signal must specify the name, data type, and causality as
parameters in separate <variable> tags under each <simulator>. Each simulator has an optional attribute decimationFactor that specifies that
simulator's decimation factor when logging variables. For example a decimation factor of 20 will lead to every 20th sample being logged. If this is
not specified, every sample will be logged. Finally, leaving out any <variable> tags on a <simulator> will lead to all variables for that simulator
being logged. Leaving out a simulator from the configuration will disable logging for that simulator. Note that this will still generate one file pr.
simulator. The log is written in CSV format only, there is currently no support for binary or other log formats.
    
### Demo co-simulation case(s)

- [DPShip](./DPShip)
- [House](./House)
- [Quarter-Truck](./Quarter-Truck)

#### Demo simulation models
[Download](./cse-demo-models)