---
title: Advanced Documentation
description: 
published: false
date: 2022-05-11T22:17:37.440Z
tags: 
editor: markdown
dateCreated: 2022-05-11T21:59:08.098Z
---

# Advanced Documentation

In this documentation you'll find information on integrating your scripts with the Power Grid system from Sonoran.



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

When a power system is disabled an event will be fired