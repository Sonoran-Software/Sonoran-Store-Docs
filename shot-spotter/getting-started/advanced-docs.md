---
title: Shot Spotter - Advanced Documentation
description: 
published: false
date: 2022-04-28T03:17:09.584Z
tags: 
editor: markdown
dateCreated: 2022-03-24T01:48:28.702Z
---

# Sonoran Shot Spotter - Advanced Documentation

## Developer Documentation
When a shot spotter is triggered it will fire the following event:
> `Sonoran:ShotSpotter:Server`

This event will pass the following data: 
| Index         | Description                                                                                                                | Example               |
|---------------|----------------------------------------------------------------------------------------------------------------------------|-----------------------|
| `serverid`       | The server ID of the player that triggered the spotter                                               | `43`             |
| `street`       | The name of the closest street to the spotter | `Vespucci Blvd.` |
| `spotter`       | The array of spotter data                                                   | See below

### Spotter Data 
| Index         | Description                                                                                                                | Example               |
|---------------|----------------------------------------------------------------------------------------------------------------------------|-----------------------|
| `Label`       | The label (name) of the spotter | Grove St. |
| `Disabled`       | The state of the shot spotter | false |
| `ID` | The ID of the shot spotter | `2`
| `Position.x` | The X coordinate | `326`
| `Position.y` | The Y coordinate | `1200`
| `Position.z` | The Z coordinate | `128`
| `Rotation.pitch` | The pitch value | `12`
| `Rotation.roll` | The roll value | `6`
| `Rotation.yaw` | The yaw value | `177`

# Example Handler
```lua
AddEventHandler("Sonoran:ShotSpotter:Server", function(serverid, street, spotter)
    print("serverid" .. serverid .. " triggered shot spotter " .. spotter.Label .. " on " .. street)
end)
```