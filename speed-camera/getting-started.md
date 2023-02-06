---
title: Speed Camera - Getting Started
description: This page will walk you through getting and installing the Speed Camera script.
published: true
date: 2023-02-06T19:27:59.881Z
tags: 
editor: markdown
dateCreated: 2022-03-31T19:23:48.740Z
---

# Getting Started

## Acquire the Script

After purchasing the script through the sonoran store you may [download the script through the keymaster account](/tebex-assets) that purchased the script. Upon downloading extract the file to a safe place.

## Install the Script

1. Inside the script package you just extracted will be two folders. Copy both to a folder in your server's resources folder called `[sonoranscripts]` note the `[]` in the name, without them it will not work.
![directory-example.png](/speed-camera/directory-example.png)
2. In the `sonoran-trafficcam` folder there will be a file called `config.CHANGEME.lua` you should rename that to be `config.lua` and configure the settings inside as you would like them to be configured based on the configuration documentation below. In that same folder will also be a file called `cameras.CHANGEME.json` which you should rename to `cameras.json` and use to manually place cameras based on the existing template, note you can also use the gun placement system in game. Finally there is a file called `discord.CHANGEME.lua` in the folder which should be renamed to `discord.lua`, this file contains all of your Discord webhook related settings.
![files-example.png](/speed-camera/files-example.png)
3. Finally, in your `server.cfg` add the following:

```
ensure sonoran-trafficcam

add_ace resource.sonoran-trafficcam command allow
add_ace resource.sonoran-trafficcam_helper command allow
```

## Configuring the Script

> As of `v2.0.2` this script utilizes two separate Lua configs. This is for security reasons. Having a separate config for your Discord Configuration keeps them server side only and safer from malicious clients {.is-danger}

Default config.lua:

```lua
Config = {}

-- General Configuration Section --
Config.configuration_version = 2.2
Config.debug_mode = false -- Only useful for developers and if support asks you to enable it
Config.permission_mode = "ace" -- Available Options: ace, framework, custom

-- Ace Permissions Section --
Config.ace_perms = {
    ace_object_place = "sonoran.trafficcam", -- Select the ace for placing new cameras
    ace_object_notification = "sonoran.police", -- Select the ace for receiving in-game notifications
    ace_object_to_place_bolo = "sonoran.bolo", -- The required ace to add BOLOs when using a framework
    ace_object_to_disable_cameras = "sonoran.disablecam" -- The required ace to disable cameras
}

-- Framework Related Settings --
Config.framework = {
    framework_type = "qb-core", -- This setting controls which framework is in use options are esx or qb-core
    police_job_names = {"police"}, -- An array of job names that should receive notifications
    allowed_to_place_groups = {"admin"}, -- The permission group that should be allowed to place new cameras
    required_grade_to_place_bolo = 4, -- The required grade to add BOLOs when using a framework
    required_grade_to_disable_cameras = 6, -- The required grade to disable cameras
    autofine = { -- This configures the autofine feature of the Speed Camera system
        enable = false,
        fine_method = "immediate", -- Options are immediate, invoice(requires the latest esx_billing for esx, invoice mode is not yet supported for QB-Core aiming for support by version 2.5.0 you can use custom mode to implement your own invoice system though), or custom(see function below)
        custom_fine = function(speed, speedlimit, playerId, fineTableFine)
            -- Put your code to fine here
        end,
        police_society_account_name = 'society_police', -- The name of the society account to pay invoices into
        fine_table = { -- Follow the format of the placeholder values, the last value in this table will be use for a speed difference of anything greater than that set value
            [1] = {
                speed_difference = 5,
                fine = 20
            },
            [2] = {
                speed_difference = 10,
                fine = 100
            },
            [3] = {
                speed_difference = 15,
                fine = 400
            },
            [4] = {
                speed_difference = 20,
                fine = 800
            },
            [5] = {
                speed_difference = 35,
                fine = 1200
            }
        }
    }
}

-- Configuration For Custom Permissions Handling --
Config.custom = {
    check_perms_server_side = true, -- If true the permission event will be sent out to the server side resource, this is recommended
    permissionCheck = function(source, type) -- This function will always be called server side.
        if type == 0 then -- Check for admin
            return true or false -- Return true if they have admin, return false if they don't
        elseif type == 1 then -- Check for notification perms
            return true or false -- Return true if they have permissions, return false if they don't
        elseif type == 2 then -- Check for BOLO creation/deletion/view perms
            return true or false -- Return true if they have permissions, return false if they don't
        elseif type == 3 then -- Check for permissions to enable/disable cameras
            return true or false -- Return true if they have permissions, return false if they don't
        end
    end
}

-- Choose Custom Command Names --
Config.command_names = {
    add_bolo_plate = "addplate",
    remove_bolo_plate = "delplate",
    spawn_new_cam = "spawnnewcam",
    cancel_new_spawn = "cancelcamplacement",
    position_debug_display = "position",
    upload_client_log = "uploadnewclientlog",
    upload_server_log = "uploadnewlog",
    change_position_data = "changepositiondata",
    show_cam_id = "showcamid",
    get_position_data = "getpositiondata",
    reload_cameras = "reloadcameras",
    list_bolo_plate = "listplates",
    disable_camera = "disablecam",
    enable_camera = "enablecam"
}

-- Settings Related to Camera Functionality --
Config.camera_settings = {
    ignore_emergency = true, -- Sets whether emergency vehicles should be ignored by the cameras
    only_ignore_on_els = true, -- If the setting above and this one are true emergency vehicles will only be ignored if their lights are on
    show_camera_blips_for_police = false, -- Should the cameras show up as blips for police on the map?
    unit_system = "mph", -- Speed unit options: mph and kph
    time_between_flags = 1, -- This is the number of minutes before a car will ping again, if they pass a different camera it will ping again regardless of whether this time has passed
    alpr_only_mode = false -- If this is true the speed detection system of this script will be disabled
}

-- Feature Settings That Don't Require Other Resources --
Config.standalone_features = {
    show_notification_blips_for_police = true, -- Should police see a blip when a car is pinged?
    blips_expire_after_seconds = 90, -- Number of seconds before the blip type above is removed
    enable_standalone_bolo_system = true, -- Selects whether the built in BOLO system should be used, this must be false for the SonoranCAD BOLO integration to work
    enable_camera_disable = true,
    enable_auto_update = true -- Should the script automatically update itself, it will check for updates regardless
}

--[[
    Placeholder list:
    {{POSTAL}}
    {{EVENT_TYPE}}
    {{PLATE}}
    {{SPEED}}
    {{SPEED_UNIT}}
    {{CAMERA_NAME}}
    {{DIRECTION}}
]]

-- Settings For Integrations With Other Resources --
Config.integration = {
    SonoranCAD_integration = {
        use = true, -- Should any of the options below be used? Integration with this script requires at least a Plus subscription.
        add_live_map_blips = true, -- Should blips for the camera be added to the live map? This requires the Pro SonoranCAD plan
        enable_911_calls = true, -- Should 911 calls be generated in the CAD when a BOLO vehicle or speeder is detected?
        ["911_caller"] = "Automated System", -- Who should the 911 call appear to be from?
        ["911_message"] = "{{EVENT_TYPE}} vehicle with license plate {{PLATE}} was seen doing {{SPEED}} {{SPEED_UNIT}} at camera {{CAMERA_NAME}}", -- Configurable 911 call description
        enable_cad_bolos = true, -- Should CAD BOLO system be used? The standalone BOLO system found above must be disabled to use this system.
        nearest_postal_plugin = "nearest-postal", -- If you want to use postals, what is the exact name of your postals script?
        disable_in_game_with_dispatch = true, -- If true disables in-game notifications when dispatch is online in CAD
        disable_cad_without_dispatch = true -- If true disables CAD notifications when dispatch is offline in CAD
    },
    SpeedLimit_Display_integration = false, -- Should the speedlimit set in the SpeedLimit Display script be used? See docs.sonoran.store for more info
}

-- Notification Settings --
Config.notifications = {
    type = "native", -- Available options: native, pNotify, okokNotify, or cadonly
    notification_title = "{{EVENT_TYPE}} Alert", -- Notification Title for methods that support it
    -- Uncomment line below and comment line 87 if you plan to use pNotify
    -- notification_message = "<b>{{EVENT_TYPE}}</b></br>License Plate: {{PLATE}}</br>Speed: {{SPEED}} {{SPEED_UNIT}}</br>Camera: {{CAMERA_NAME}}"
    notification_message = "{{EVENT_TYPE}} Alert\nLicense Plate: {{PLATE}}\nSpeed: {{SPEED}} {{SPEED_UNIT}}\nCamera: {{CAMERA_NAME}}" -- The text of the notification
}
```

### Note
In order for auto fines to function properly, Config.permission_mode **MUST** be "framework"

```lua
DiscordConfig = {
		enabled = false, -- Should discord webhooks be used?
    webhook_url = "", -- See https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks
    webhook_title = "{{EVENT_TYPE}} Alert", -- The title of the webhook embed
    webhook_message = "License Plate: {{PLATE}}\nSpeed: {{SPEED}} {{SPEED_UNIT}}\nCamera: {{CAMERA_NAME}}" -- The message that the webhook displays
}
```

## Setting Up Permissions (Ace Permissions Only)

To use this script you must assign the permissions object to the groups which you would like to have permissions. You can find the objects needed in the config under `Config.ace_perms` section in the config. To assign these perms you must add the lines to your `server.cfg` or where ever you setup permissions on your server. You can learn more about Ace Permissions [here](https://forum.cfx.re/t/basic-aces-principals-overview-guide/90917).

Example:
```lua
add_ace group.admin sonoran.trafficcam allow
add_ace group.leo sonoran.police allow
add_ace group.leosupervisor sonoran.bolo allow
add_ace group.moderator sonoran.disablecam allow
```

## Camera Location Config

You have two options for placing new cameras:

1. You can use the command `/spawnnewcam [prop] [name]` to initiate spawning a new camera and generate the relevant config data
    - After running this command you must pull out, aim, and shoot with a gun to confirm placement.
    - You may need to modify some of the rotation values manually to get that perfect placement you are looking for.
    - If you aren't using the speed limit display integration you will need to manually go into the config file to change the set speed limit. You will also need to change the view radius to be the way you want it.
    - [Click here for more information.](/gun-placement)
2. You can manually copy and paste an existing config and then modify the values to meet your needs for the new camera

### `camera.json` Property Explanation

| Property Name | Example   | Notes                                                                |
| ------------- | --------- | -------------------------------------------------------------------- |
| `ID`          | 2         | `ID` must be unique. No other camera can share this ID               |
| `Prop`        | `radar01` | Valid values are `radar01` and `porp_traffic_cam`                    |
| `Position`    |           | This is a table that contains the x, y, and z coords of the camera   |
| `Rotation`    |           | This is a table that contains the x, y, and z rotation of the camera |
| `Label`       | `Test 1`  | This is a human readable label for the camera, can have spaces       |
| `SpeedLimit`  | 10        | This is the speed over which this camera will trip                   |
| `ViewRadius`  | 10        | How far away this camera can see in GTA units, default is 10         |

## Commands
| Command Name | Command Description | Required Permission |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/spawnnewcam [prop] [name]` | This command will allow an admin to spawn a new camera using a gun placement system where the name argument is the label for the new camera and the prop argument is the text name of the model to use | Admin or as configured |
| `/addplate [plate]` | This command will add a plate to the standalone BOLO system when in use. | LEO or as configured |
| `/delplate [plate]` | This command will remove a plate from the standalone BOLO system when in use. | LEO or as configured |
| `/listplates` | This command will list the plates currently configured in the standalone BOLO system when in use | LEO or as configured |
| `/showcamid` | This command will draw the ID of cameras which you are in the radius of with 3D text near the camera | N/A |
| `/getpositiondata` | This command will print the current positional data of the camera to the chat | Admin or as configured |
| `/changepositiondata` | This command will change the value which you specify of the camera you specify, run this command without arguments for example usage. All changes made with this command will be immediately saved | Admin or as configured |
| `/reloadcameras` | This command will completely reload the cameras.json from the server's storage | Admin or as configured |
| `/disablecamera` | This command will disable the camera with the ID specified as an argument, this will prevent that camera from flagging vehicles | LEO or as configured |
| `/enablecamera` | This command will enable the camera with the ID specified as an argument, this resume that camera's ability to flag vehicles | LEO or as configured |
| `/cancelcamplacement` | This command will cancel the current camera placement if one is currently in progress. | N/A |

## Model Options
![promo-models.png](/speed-camera/promo-models.png)
The model on the left is named `prop_traffic_cam` and the model is named `radar01`.

## Needed Changes for Speed Display

We recommend [this](https://forum.cfx.re/t/release-posted-speedlimit/180949) speed limit display script by BigYoda.

Add the following code snippet to the bottom of your speed display script's client.lua:

```lua
exports("GetCurrentSpeed", function()
    return speedlimit
end)
```
