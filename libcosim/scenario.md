---
layout: default
title: "Scenario"
permalink: /libcosim/scenario
has_toc: false
parent: "libcosim"
nav_order: 2
---
# Scenario

A scenario file defines the simulation scenario which shall be performed. This serves for dynamic systems where actions to be made to trigger any event, activate models or sending signals to the controllers, etc. The example below shows the syntax structure of a scenario file. First is the `"description"` of what does this scenario do. 
`"action"` declares the action type which can be `"override"` , `"bias"`  or `"reset"`. `"events"` includes the `"time"` in second when the action takes place, on which `"model"`, the "`variable"` name in this model and the `"value"` for this variable (except for `"reset"` action type which doesn't require a value) .

```json
{ "description": "description of the scnario",
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
