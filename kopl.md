---
layout: default
title: "Configuration tool"
permalink: /kopl
has_toc: false
nav_order: 7
---

# Configuration tool
`Kopl` is a configurator to help set up and run a simulation. It can also be used to validate the model using the [OSP validator](./osp-validator). 
`Kopl` is not an open source software but can be freely used without any restrictions.

{% include figure.html 
    img="/assets/img/Kopl.png" 
    num="1" 
    caption="Kopl configurator" 
%}

#### Short introduction video

<video src="/assets/videos/Kopl.mp4" width="640" height="400" controls preload></video>


## How to get started

* Get the appropriate version of `Kopl` [here](./downloads).
* Unzip somewhere on disk and double click the `Kopl` executable
* Choose a location for the workspace (working folder) or just hit OK to use the default workspace
* Right click the navigator area to create a new CoSimulation Task. Give it a name or leave the default
* Create a new configuration which represents a simulation.  Right click -> New configuration. 
Here you have two options,either to create an empty configuration or point to an existing .ssd or open system structure xml file and import this.
* Expand the newly created configuration and double click the setup. This opens up the diagram for connecting several FMUs together.
* Use the palette on the right hand side to drag and drop components onto the diagram

## Adding a new FMU
* It is possible to drag an FMU from the file explorer onto the diagram or drag the FMU element from the Components menu in the Palette.
* When an FMU block is added to the diagram, you can double click it to open the editor for it.
* When creating a FMU block from the palette you need to point to the fmu on disk. 
* Press the "load variables" action to read the model description within the FMU. The relevant variables should then be shown in the definitions tab
* It is now possible to make variables available as input or output. Press the plus button and select the wanted variable to use as input/output within the under the "Input" or "output" tabs.

## Connecting the FMUs
* Once two or more FMUs are added to the diagram it is possible to add connections between the blocks. 
Just start dragging from an output or input slot and drop when hovering over the correct terminal.

## Variable groups (OSP-IS)
* If you want to follow the OSP-IS, you can start creating variable groups in the definitions tab within the variable groups tab.
* Click the green button or right click an empty space in the local navigator/tree widget
* Created variable groups may then be choosen as input or output to the FMU

{% include figure.html 
    img="/assets/img/Kopl-variablegroups.png" 
    num="2" 
    caption="Definition of nested variable groups" 
%}

## OSP-IS validation

Right click the configuration and choose validate in the context menu to let the [OSP validator](./osp-validator) validate the current setup

## Export

Right click the configuration and choose export in the context menu, then choose between the different formats.

## Simulation

Right click the configuration and choose "Simulation" in the context menu, or run simulation in the diagram.
This will export the setup and use the CLI behind the scene. The CLI version can be switched (currently necessary on the Linux version) in the preferences.
See main menu->Tools->Preferences->CoSim configuration.
When the simulation is done you should be able to see the results in the navigator and basic ploting is available.

{% include figure.html 
    img="/assets/img/Kopl-results.png" 
    num="3" 
    caption="Results after a simulation is done" 
%}