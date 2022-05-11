---
title: Advanced Documentation
description: 
published: true
date: 2022-05-11T22:50:51.355Z
tags: 
editor: markdown
dateCreated: 2022-05-11T21:59:08.098Z
---

# Advanced Documentation

In this documentation you'll find information on integrating your scripts with the Power Grid system from Sonoran.

> All events listed here are server side {.is-warning}

### Registration Event

This event will be called when a player with admin permissions runs the `/link` command to link a system to a new device. The event will include the position and entity id of the device as well as the request ID as parameters. When receiving this information you should send back the data about the object either near/at that location or with that entity ID. See examples below for details.

```lua
AddEventHandler("SonoranScripts::PowerGrid::RegisterNewDevice", function(coords, entityID, requestID)
	-- Get the correct system
  TriggerEvent("SonoranScripts::PowerGrid::NewDevice", "internal device identifier", "internal script identifier", requestID)
end)
```

Below is an example of how registration is done within our Traffic Camera Script:
```lua
AddEventHandler("SonoranScripts::PowerGrid::RegisterNewDevice", function(coords, entityID, requestID)
	for _, v in pairs(cameraDatabase) do
  	if v.entityObject == entityID then
    	TriggerEvent("SonoranScripts::PowerGrid::NewDevice", v.ID, "trafficcams", requestID)
    end
  end
end)
```

When a power system is disabled an event will be triggered that contains a table of internal script identifiers that map to a table of internal IDs of devices that are affected by the system that was disabled. An example from the Sonoran Traffic Camera script is available below:
```lua
AddEventHandler("SonoranScripts::PowerGrid::DeviceDisabled", function(affectedDevices)
	for _, v in pairs(affectedDevices["trafficcams"]) do
  	cameraDatabase[tostring(v.ID)].disabled = true
  end
  TriggerClientEvent(GetCurrentResourceName() .. "::UpdateDB", -1, cameraDatabase)
end)
```

When a power system is repaired a similar event will be triggered an example from the same script is below:
```lua
AddEventHandler("SonoranScripts::PowerGrid::DeviceRepaired", function(affectedDevices)
	for _, v in pairs(affectedDevices["trafficcams"]) do
  	cameraDatabase[tostring(v.ID)].disabled = false
  end
  TriggerClientEvent(GetCurrentResourceName() .. "::UpdateDB", -1, cameraDatabase)
end)
```