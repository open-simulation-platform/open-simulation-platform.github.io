---
layout: default
title: "Scenario"
permalink: /cse-core/scenario
has_toc: false
parent: "libcosim"
nav_order: 2
---
# Scenario

A scenario file defines the simulation scenario which shall be performed. This serves for dynamic systems where actions to be made to trigger any event, activate models or sending signals to the controllers, etc. The example below shows the syntax structure of a scenario file.

| attribute       | Description                                                                                                                              |
| :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| `description`           | Commenting description of the simulation scenario.  |
| `model`         | Declare which model the events are triggered.|
| `action`        | Declares the action type which can be "override" , "bias" or "reset".|
| `events`       | Contains all actions of this scenario including the time, the updated variable and the new value.|
| `time`        | Declare the time in second when the action takes place.|
| `variable`        | Declare the variable name to be updated at this event.|
| `value`        | Declare the updated value of the variable, "true" or "false" if it's a boolean type of variable. Action type "reset" doesn’t require value for the variables.|
| `end`        | Declare the stop time in seconds of the scenario. Naturally the stop time should be bigger than the last event time.|

```json
{ "description": "description of the scenario",
  "defaults": 
  {
    "model": "model1",
    "action": "override"
  },
  "events": 
  [
    {
      "time": 50,
      "variable": "variable1",
      "value": true
    },
    {
      "time": 60,
      "variable": "variable2",
      "value": 1
    },
    {
      ...      event3       ...
    }
  ],
  "end": 100
}
```
