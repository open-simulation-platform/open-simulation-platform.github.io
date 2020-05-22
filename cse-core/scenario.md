---
layout: default
title: "Scenario"
permalink: /cse-core/scenario
has_toc: false
parent: "libcosim"
nav_order: 2
---
# Scenario

A scenario file defines the simulation scenario which shall be performed. This serves for dynamic systems where actions to be made to trigger any event, activate models or sending signals to the controllers, etc. The example below shows the syntax structure of a scenario file, which can be in the format of .json or .yml.

| data       | element | Description                                                                                                                              |
| :--------------- | :---------------| :-------------------------------------------------------------------------------------------------------------------------------------- |
| `description`    |        | Commenting description of the simulation scenario.  |
| `defaults`      |       | Contains the default elements for the events, for example `model` `variable` and `action`, where all events apply to this variable in this model and have the same action type. Alternatively, the `model`, `variable` and `action` elements can be included in individual event. |
| |    `model`    | Declares which model the events are triggered.|
| |`action`        | Declares the action type which can be "override" , "bias" or "reset".|
| |`variable`        | Declares the variable name to be updated at this event.|
| `events`   |    | Contains all actions of this scenario including the time, the updated variable and the new value.|
| |`time`        | Declares the time in second when the action takes place.|
| | `value`        | Declares the updated value of the variable, "true" or "false" if it's a boolean type of variable. Action type "reset" doesn’t require value for the variables.|
|   |   `end`   | Declares the stop time in seconds of the scenario. Naturally the stop time should be bigger than the last event time.|

**Scenario.json**
```json
{ "description": "description of the scenario",
  "defaults": {
    "model": "slave uno",
    "variable": "realOut",
    "action": "override"
  },
  "events": [
    {
      "time": 0.5,
      "variable": "realIn",
      "action": "bias",
      "value": 1.001
    },
    {
      "time": 0.2,
      "value": -1.0
    },
    {
      "time": 0.3,
      "action": "reset"
    },
    {
      "time": 0.65,
      "variable": "intIn",
      "value": 2
    },
    {
      "time": 0.8,
      "variable": "intOut",
      "value": 5
    }
  ],
  "end": 1.0
}
```

**Scenario.yml**
```yaml
---
defaults:
  model: slave uno
  variable: realOut
  action: override
events:
  - time: 0.5
    variable: realIn
    action: bias
    value: 1.001
  - time: 0.2
    value: -1
  - time: 0.3
    action: reset
  - time: 0.65
    variable: intIn
    value: 2
  - time: 0.8
    variable: intOut
    value: 5
end: 1
```