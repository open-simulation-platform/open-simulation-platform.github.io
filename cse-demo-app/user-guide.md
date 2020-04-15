---
layout: default
title: "user-guide"
permalink: /cse-demo-app/user-guide
---

## User guide

### Download and install 

Antivirus alert: Be aware that your virus protection software might react to the cse application. It's not harmful and you can allow it to run.

- Windows
1. Download the latest CSE demo application. cse-vX.Y.Z-win64.zip
2. Extract the files and you will have a root folder looking like below
3. Download dp-ship example models: dp-ship.zip
4. Extract the demo models, for instance next to the cse-vX.Y.Z-win64.zip folder
5. Run startup script: run-windows.cmd
6. The application should open in your web browser at url http://localhost:8000.

![foo](/assets/img/CSEuserguideFig1.png "Figure 1")

- Linux
1. Download CSE demo application archive: cse-server-go-vX.Y.Z.tar.gz
2. Extract archive: tar -xzvf cse-server-go-vX.Y.Z.tar.gz
3. Download dp-ship example models: dp-ship.zip
4. Extract the demo models, for instance next to the cse-vX.Y.Z-win64 folder
5. Run startup script: run-linux
6. The application should open in your web browser at url http://localhost:8000.

### Loading Configuration
1. Switch to your web browser at url http://localhost:8000.
2. Enter the path to the dp-ship folder on your machine.
3. Enter the path to a directory where you want the logs to be written. Leave it blank to disable file logging.
4. Previously used configurations are available below.
5. Click "Load simulation" to load the configuration.

![foo](/assets/img/CSEuserguideFig2.png "Figure 2")

### Models and variables
1. The models in the simulation will appear on the left hand side.
2. Browse the model variables by clicking the names. Their variables are organized in tabs based on causality.
3. The simulation starts when clicking "Play".

![foo](/assets/img/CSEuserguideFig3.png "Figure 3")

### Simulation control
1. Click "Simulation status" to see details about the simulation and to access some simulation controls.
2. By clicking "Enable" on the real time target, the simulation will run in real time. If real time target is disabled, the simulation will run as fast
as possible.
3. RTF (Real time factor) indicates simulation speed related to real time. RTF = 2 means that simulation runs twice the speed of real time.
4. Real time factor target value can be altered to set a specific RTF.
5. Click Play/Pause to start or pause the simulation.
6. To close the current simulation for loading another configuration, click the "Tear down" button.

![foo](/assets/img/CSEuserguideFig4.png "Figure 4")

### Plotting variables
1. Click "Create new time series" or "Create new XY plot" to obtain a time series plot or an XY plot that variables can be added to.
2. Any variable can be added to a plot by clicking "Add to plot" in the variable overview.
3. Select where to plot the variable from the list of available plots.
4. Click the plot names to see the charts.

![foo](/assets/img/CSEuserguideFig5.png "Figure 5")
![foo](/assets/img/CSEuserguideFig6.png "Figure 6")

### Overriding variables

It is possible to override any input, output or parameter variable.
1. Click the edit icon to the left of the variable.
2. Type in the value and click the check icon.
3. Click the eraser symbol to remove the override.

![foo](/assets/img/CSEuserguideFig7.png "Figure 7")

### Scenarios

1. Scenario files placed in the ./scenarios subfolder within the configuration folder are automatically loaded and made visible in the
Scenarios section.The dp-ship example comes with three scenario files for demonstration purposes.
2. Click the scenarios to see its contents.
3. To execute a scenario click "Load scenario" before starting the simulation.

![foo](/assets/img/CSEuserguideFig9.png "Figure 9")

### Co-simulation configuration

The dp-ship, quarter-truck and house examples each contains a
OspSystemStructure.xml file that demonstrates how this is done.

```
<?xml version="1.0" encoding="UTF-8"?>
<OspSystemStructure
xmlns="http://opensimulationplatform.com/MSMI/OSPSystemStructure">
    <BaseStepSize>0.01</BaseStepSize>
    <Simulators>
        <Simulator name="ModelX" source="fmu1.fmu">
            <InitialValues>
                <InitialValue variable="input1">
                    <Real value="42"/>
                </InitialValue>
            </InitialValues>
        </Simulator>
        <Simulator name="ModelY" source="fmu2.fmu" stepSize="0.1"/>
        <Simulator name="ModelZ" source="fmu3.fmu" stepSize="0.05"/>
    </Simulators>

    <PlugSocketConnections/>
    <BondConnections/>
    <VariableConnections>
        <ScalarConnection>
            <Source simulator="ModelX" endpoint="p.out"/>
            <Target simulator="ModelY" endpoint="p.in"/>
        </ScalarConnection>
    </VariableConnections>
</OspSystemStructure>
```

The examples each contains a SystemStructure.ssd file that demonstrates configuration using the SSP standard.
CSE currently supports using a fixed step algorithm. The step size can be altered in the SystemStructure.ssd file.
The new MSMI connection types are not supported when using the SSP standard.

### Configurable logging
In order to log signal values from a simulation to file, an output directory must be specified in the "Log folder" field on the simulation setup. By
default, all signals will be logged and persisted on every sample. There will be one file generated per simulator. This can quickly lead to a large
amount of data being generated, so we recommend instead specifying what signals to log using the configurable log format outlined below.
CSE supports basic configuration of specific signals to log from any simulator via an XML file. This file must be named "LogConfig.xml" (exactly
including case) and placed in the same folder as the simulators. A basic example of the currently supported syntax is;

```
<simulators>
    <simulator name="model_1" decimationFactor="20">
        <variable name="model_input"/>
        <variable name="model_output"/>
    </simulator>
    <simulator name="model_2" decimationFactor="10">
        <variable name="model_input"/>
        <variable name="model_output"/>
    </simulator>
        <simulator name="model_3"/>
</simulators>
```

The simulators to be logged must be enclosed in a <simulators> tag, and each signal must specify the name, data type, and causality as
parameters in separate <variable> tags under each <simulator>. Each simulator has an optional attribute decimationFactor that specifies that
simulator's decimation factor when logging variables. For example a decimation factor of 20 will lead to every 20th sample being logged. If this is
not specified, every sample will be logged. Finally, leaving out any <variable> tags on a <simulator> will lead to all variables for that simulator
being logged. Leaving out a simulator from the configuration will disable logging for that simulator. Note that this will still generate one file pr.
simulator. The log is written in CSV format only, there is currently no support for binary or other log formats.

### Plot
Two types of plot are supported by the cse-demo-application, namely **trend** and **scatter**. The trend type shows the curve of a variable over time, while the scatter type shows the relation between two variables, of one versus the other.     
An example is shown below. This can be pre-defined and loaded into the cse-demo-app like the scenario file. It is also possibel to add through the demo-app user interface editor. More details and examples can be found in the demo-app descriptions. 
```
{
  "plots": [
    {
      "label": "label",
      "plotType": "trend",
      "variables": [
        {
          "simulator": "model name 1",
          "variable": "variable name 1"
        },
        {
          "simulator": "model name 2",
          "variable": "variable name 2"
        }
      ]
    },
    {
      "label": "Room temp XY",
      "plotType": "scatter",
      "variables": [
        {
          "simulator": "model name 1",
          "variable": "variable name 1"
        },
        {
          "simulator": "model name 2",
          "variable": "variable name 2"
        }
      ]
    }
  ]
}
```
