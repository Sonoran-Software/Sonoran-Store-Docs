---
title: Getting Started
description: This page will walk you through getting and installing the script.
published: true
date: 2022-04-22T21:41:11.536Z
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
2. In the `sonoran-trafficcam` folder there will be a file called `config.CHANGEME.json` you should rename that to be config.json and configure the settings inside as you would like them to be configured based on the configuration documentation below. In that same folder will also be a file called `cameras.CHANGEME.json` which you should rename to `cameras.json` and use to manually place cameras based on the existing template, note you can also use the gun placement system in game.
![files-example.png](/speed-camera/files-example.png)
3. Finally, in your `server.cfg` add the following:
```
ensure sonoran-trafficcam

add_ace resource.sonoran-trafficcam command allow
add_ace resource.sonoran-trafficcam_helper command allow
```

## Configuring the Script
Default config.json:
```lua
Config = {}

-- General Configuration Section --
Config.configuration_version = 2.0
Config.debug_mode = false -- Only useful for developers and if support asks you to enable it
Config.permission_mode = "ace" -- Available Options: ace, framework, custom

-- Ace Permissions Section --
Config.ace_perms = {
    ace_object_place = "sonoran.trafficcam", -- Select the ace for placing new cameras
    ace_object_notification = "sonoran.police" -- Select the ace for receiving in-game notifications
}

-- Framework Related Settings --
Config.framework = {
    framework_type = "qb-core", -- This setting controls which framework is in use options are esx or qb-core
    police_job_names = {"police"}, -- An array of job names that should receive notifications
    allowed_to_place_groups = {"admin"} -- The permission group that should be allowed to place new cameras
}

-- Configuration For Custom Permissions Handling --
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

-- Choose Custom Command Names --
Config.command_names = {
    add_bolo_plate = "addplate",
    remove_bolo_plate = "delplate",
    spawn_new_cam = "spawnnewcam",
    cancel_new_spawn = "cancelcamplacement",
    position_debug_display = "position",
    upload_client_log = "uploadnewclientlog",
    upload_server_log = "uploadnewlog"
}

-- Settings Related to Camera Functionality --
Config.camera_settings = {
    ignore_emergency = true, -- Sets whether emergency vehicles should be ignored by the cameras
    only_ignore_on_els = true, -- If the setting above and this one are true emergency vehicles will only be ignored if their lights are on
    show_camera_blips_for_police = false, -- Should the cameras show up as blips for police on the map?
    unit_system = "mph", -- Speed unit options: mph and kph
    time_between_flags = 1 -- This is the number of minutes before a car will ping again, if they pass a different camera it will ping again regardless of whether this time has passed
}

-- Feature Settings That Don't Require Other Resources --
Config.standalone_features = {
    show_notification_blips_for_police = true, -- Should police see a blip when a car is pinged?
    blips_expire_after_seconds = 90, -- Number of seconds before the blip type above is removed
    enable_standalone_bolo_system = true, -- Selects whether the built in BOLO system should be used, this must be false for the SonoranCAD BOLO integration to work
    enable_auto_update = true -- Should the script automatically update itself, it will check for updates regardless
}

-- Settings For Integrations With Other Resources --
Config.integration = {
    SonoranCAD_integration = {
        use = true, -- Should any of the options below be used?
        add_live_map_blips = true, -- Should blips for the camera be added to the live map? This reqires the Pro SonoranCAD plan.
        enable_911_calls = true, -- Should 911 calls be generated in the CAD when a BOLO vehicle or speeder is detected?
        ["911_caller"] = "Automated System", -- Who should the 911 call appear to be from?
        ["911_message"] = "{{EVENT_TYPE}} vehicle with license plate {{PLATE}} was seen doing {{SPEED}} {{SPEED_UNIT}} at camera {{CAMERA_NAME}}", -- Configurable 911 call description
        enable_cad_bolos = true, -- Should CAD BOLO system be used?
        nearest_postal_plugin = "nearest-postal" -- If you want to use postals, what is the exact name of your postals script?
    },
    SpeedLimit_Display_integration = false, -- Should the speedlimit set in the SpeedLimit Display script be used? See docs.sonoran.store for more info
    Discord_Webhook = {
        enabled = false, -- Should discord webhooks be used?
        webhook_url = "", -- See https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks
        webhook_title = "{{EVENT_TYPE}} Alert", -- The title of the webhook embed
        webhook_message = "License Plate: {{PLATE}}\nSpeed: {{SPEED}} {{SPEED_UNIT}}\nCamera: {{CAMERA_NAME}}" -- The message that the webhook displays
    }
}

-- Notification Settings --
Config.notifications = {
    type = "native", -- Available options: native, pNotify, okokNotify, or cadonly
    notification_title = "{{EVENT_TYPE}} Alert", -- Notification Title for methods that support it
    -- Uncomment line below and comment line 84 if you plan to use pNotify
    -- notification_message = "<b>{{EVENT_TYPE}}</b></br>License Plate: {{PLATE}}</br>Speed: {{SPEED}} {{SPEED_UNIT}}</br>Camera: {{CAMERA_NAME}}"
    notification_message = "License Plate: {{PLATE}}\nSpeed: {{SPEED}} {{SPEED_UNIT}}\nCamera: {{CAMERA_NAME}}" -- The text of the notification
}
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
|---------------|-----------|----------------------------------------------------------------------|
| `ID`          | 2         | `ID` must be unique. No other camera can share this ID               |
| `Prop`        | `radar01` | The only valid value for `Prop` is currently `radar01`               |
| `Position`    |           | This is a table that contains the x, y, and z coords of the camera   |
| `Rotation`    | 				  | This is a table that contains the x, y, and z rotation of the camera |
| `Label`       | `Test 1`  | This is a human readable label for the camera, can have spaces       |
| `SpeedLimit`  | 10        | This is the speed over which this camera will trip                   |
| `ViewRadius`  | 10        | How far away this camera can see in GTA units, default is 10         |

## Needed Changes for Speed Display
We recommend [this](https://forum.cfx.re/t/release-posted-speedlimit/180949) speed limit display script by BigYoda.

Add the following code snippet to the bottom of your speed display script's client.lua:
```lua
exports("GetCurrentSpeed", function()
    return speedlimit
end)
```