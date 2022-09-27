---
title: Evidence Camera - Advanced Documentation
description: Developer Documentation and other Advanced Configuration Topics for the Evidence Camera
published: true
date: 2022-09-27T18:27:12.436Z
tags: 
editor: markdown
dateCreated: 2022-09-27T18:27:12.436Z
---

# Advanced Documentation	 

## Custom Inventory Integration 
If you wish to use a custom inventory please first ensure that `use_custom_inventory` is set to `true` in your `config.lua`

For all the handling of photo items please see the `/server/inventory.lua` file. In this file it is recommended that do the following:

#### Define your framework. Ex. Below
```lua
local QBCore = nil
if config.frameworks.use_qbcore then
    QBCore = exports["qb-core"]:GetCoreObject()
end
```

#### Begin your handling (All examples will be shown using QBCore)
First we will register our item with the framework. Ex. Below
```lua
        exports["qb-core"]:AddItem("sonoran_evidence_photo", {
            name = "sonoran_evidence_photo",
            label = "Photo",
            weight = 0,
            type = "item",
            image = "evidence.png",
            unique = true,
            useable = true,
            shouldClose = false,
            combinable = nil,
            description = "A sweet polaroid photo"
        })
        QBCore.Functions.CreateUseableItem("sonoran_evidence_photo", function(source, item)
            TriggerClientEvent("sonoran:lookphoto:qbcore", source, item)
        end)
```

Next we will make the item usable. Ex. Below
```lua
        QBCore.Functions.CreateUseableItem("sonoran_evidence_photo", function(source, item)
            TriggerClientEvent("sonoran:lookphoto:qbcore", source, item)
        end)
```

Now that our item is registered and usable we will begin the handling. The following event will be triggered when the photo is first put away. You will want to use this to cache the images link. `Sononran:PutAway:Custom:First`. The example handling will be used in the form of QBCore. See below for example. 
```lua
    RegisterNetEvent("Sononran:PutAway:Custom:First", function(image)
        local Player = QBCore.Functions.GetPlayer(source)
        if not Player then
            return
        end
        local info = {}
        info.image_link = image
        Player.Functions.AddItem("sonoran_evidence_photo", 1, nil, info)
    end)
```

### Passed Parameters To `Sononran:PutAway:Custom:First`
| Parameter Name | Parameter Description
| --- |--- 
| `image` | The direct image link