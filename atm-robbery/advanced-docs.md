---
title: ATM Robbery - Advanced Documentation
description: Developer Documentation and other advanced configuration options of the ATM Robbery system.
published: true
date: 2023-01-03T23:13:44.457Z
tags: 
editor: markdown
dateCreated: 2022-12-22T03:37:16.587Z
---

# Advanced Documentation

## Custom Permission Systems

If you view your `config.lua` you will find a function to be defined with your permissions check. The same function is called to check for permission to receive notifications and to check if they are allowed to place new ATMs. The requests are differentiated through the type argument. It will be equal to `0` if the permissions check is to see if they can place a ATM, `1` to check if they can currently receive notifications, or `2` to check if they are allowed to initiate the stealing of an ATM, The permission check will be called on every attempted placement, every notification sent, and every attempted robbery.

```lua
Config.custom = {
    check_perms_server_side = true, -- If true the permission event will be sent out to the server side resource, this is recommended
    permissionCheck = function(source, type) -- This function will always be called server side.
        if type == 0 then -- Check for admin
            return true or false -- Return true if they have admin, return false if they don't
        elseif type == 1 then -- Check for notification perms
            return true or false -- Return true if they have permissions, return false if they don't
        elseif type == 2 then -- Check for perms to steal the ATM
            return true or false -- Return true if they have permissions, return false if they don't
        end
    end
}
```

If you need assistance setting this up you can hire a dev by going [here](https://support.sonoransoftware.com/#/) and selecting "Hire a Developer".

## Developer Documentation

When an ATM is being drilled initially, the `sonoran-atmrobbery::ATM::Drilling_s` event will fire to the `server`. The data table can be seen below. Upon completion of the final drilling, if `Config.permissionMode` is set to `ace`, the event `Sonoran::ATM::Payout` will be fired to the `server`. The data table can be seen below. Example event handlers can also be found below.
#### Event List

-   `Sonoran::ATM::Payout`
- `sonoran-atmrobbery::ATM::Drilling_s`

#### Data Table Structure For `Sonoran::ATM::Payout`

| Index         | Description                                                                                                                | Example               |
| ------------- | -------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| `money`       | This will be the payout amount recieved from drilling into the ATM                                               | `14546`             |
| `type`       | This will be the type of money from the `Config.framework.reward.rewardType` | `cash` |
| `items`       | This will contain the items the ATM gives out after drilling. Configurable in the `Config.framework.reward.Items`                                                   | `steel`              |

#### Data Table Structure For `sonoran-atmrobbery::ATM::Drilling_s`

| Index         | Description                                                                                                                | Example               |
| ------------- | -------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| `street`       | This will be the street name the robbery is taking place on                                               | `Mirror Park Blvd`             |
| `postal`       | This will be the postal where the robbery is happening | `7054` |
| `location` | This will be the vector3 coordinates of where the ATM is being stolen | `"x":124, "y": 453, "z":30`
| `atm` | This will be the hash of the ATM that is being stolen | `-156843`

#### Example Handler

```lua
AddEventHandler("Sonoran::ATM::Payout", function(data)
	print("Player 1 recieved $" .. data["money"] .. " in " .. data["type"] .. " along with " .. data["items"].amount .. " " .. data["items"].item)
end)
```

```lua
AddEventHandler("sonoran-atmrobbery::ATM::Drilling_s", function(data)
	print("An ATM with the hash " .. data["atm"] .. " is currently being stolen on " .. data["street" .. " (" .. data["postal"] .. "). Exact coordinates: ".. data["location"])
end)
```

