---
title: Speed Cameras - Advanced Documentation
description: 
published: true
date: 2022-04-23T00:27:20.766Z
tags: 
editor: markdown
dateCreated: 2022-04-23T00:23:30.363Z
---

# Advanced Documentation

## Custom Permission Systems
If you view your `config.lua` you will find a function to be defined with your permissions check. The same function is called to check for permission to receive notifications and to check if they are allowed to place new cameras. The requests are differentiated through the type argument. It will be equal to `0` if the permissions check is to see if they can place a camera, and `1` to check if they can currently receive notifications. The permission check will be called on every attempted placement and every notifcation sent, so you can use a custom "on-duty" system if desired.
```lua
Config.custom = {
    check_perms_server_side = true, -- If true the permission event will be sent out to the server side resource, this is recommended
    permissionCheck = function(source, type) -- This function will always be called server side.
        if type == 0 then -- Check for admin
            return true or false -- Return true if they have admin, return false if they don't
        elseif type == 1 then -- Check for notification perms
            return true or false -- Return true if they have permissions, return false if they don't
        end
    end
}
```

If you need assitance setting this up you can hire a dev by going [here](https://support.sonoransoftware.com/#/) and selecting "Hire a Developer".

## Developer Documentation

When a BOLO vehicle is caught events will be sent out. It will be sent to both the client that was caught and the server. It will contain a data argument, the table below contains the layout of this value. Events are named the same on both the client and server.

#### Event List
- `SonoranScripts::PlateBOLO` (Used if standalone BOLO system is used)
- `SonoranScripts::SPlateBOLO` (Used if SonoranCAD BOLO system is used)
- `SonoranScripts::Speeding`

#### Data Table Structure
| Index         | Description                                                                                                                | Example               |
|---------------|----------------------------------------------------------------------------------------------------------------------------|-----------------------|
| `plate`       | This will contain the license plate of the vehicle that was seen as a string                                               | `24EDT43`             |
| `color`       | This will contain the color of the vehicle as a string. Primary and secondary color of the vehicle are separated by a `/`. | `Util Red / Worn Red` |
| `model`       | This will contain the model's text name as it is set in the vehicle.meta                                                   | `Blista`              |
| `speed`       | The vehicles speed to the nearest whole number                                                                             | 25                    |
| `camera_name` | The name of the camera as a string                                                                                         | `Los Santos Freeway`  |
| `camera_id`   | The numerical id of the camera                                                                                             | 4                     |
| `postal`      | If the postal script is enabled, this will contain the nearest postal of the sighting                                      | 413                   |

#### Example Handler
