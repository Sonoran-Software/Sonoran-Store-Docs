---
title: Getting Started
description: This page will walk you through getting and installing the script.
published: false
date: 2022-04-05T23:20:09.472Z
tags: 
editor: markdown
dateCreated: 2022-03-31T19:23:48.740Z
---

# Getting Started
## Acquire the Script
After purchasing the script through the sonoran store you may download the script through the keymaster account that purchased the script. Upon downloading extract the file to a safe place.

## Install the Script
Inside the script package will be a file called config.CHANGEME.json you should rename that to be config.json. You can then install the script as you would any other resource for FiveM.

## Configuring the Script
Default config.json:
```js
{
	"configuration_version": 1.0, //An internal identifier for the version number
  "script_version": 1.0, //An internal identifier for the script version
  "debugMode" = false, //Debug mode may cause console spam, useful for debugging issues
	"ace_perms": {
		"use_ace": false, //Change to true to use the FiveM Ace Perms system
		"ace_object_place": "sonoran.trafficcam", //The name of the permission to place new traffic cameras
		"ace_object_notification": "sonoran.police" //The name of the permission to receive notifications and to manage the BOLO system
	},
	"framework": {
		"framework_enabled": true, //This is used to choose whether or not to use a framework such as ESX or qb-core
		"framework_type": "qb-core", //Change to ESX if that is your framework
		"police_job_names": ["police"], //A list of police jobs to receive notifications and manage the BOLO system
		"allowed_to_place_groups": ["admin"] //A list of QBCore permission groups allowed to add new cameras
	},
	"custom": {
		"use_custom": false, //Set to true if you want to setup a custom permission syste
		"check_perms_server_side": true, //Choose to verify custom perms on the server or client (server is recommended for security)
		"custom_place_event": "community::IsAdmin", //The name of the event that should be triggered when checking if someone can place new cameras
		"custom_notify_event": "community::IsPolice" //The name of the event that should be triggered when checking if someone should receive notifications
	},
	"camera_settings": {
		"ignore_emergency": true, //Configure whether the speed cameras should ignore emergency vehicles
		"only_ignore_on_els": true, //Configure whether emergencies vehicles should only be ignored if they are running lights (only works if above value is set to true)
		"show_camera_blips_for_police": false, //Should camera blips be shown on officer's in-game maps
		"unit_system": "mph", //What unit system should be used, "mph" and "kph" are the only valid option
		"time_between_flags": 1 //The number of minutes between alerts about a single vehicle at a location, this will be overridden if the vehicle is seen by a different camera before this time has passed.
	},
	"standalone_features": {
		"show_notification_blips_for_police": true, //Set to false to hide deteted vehicles map blips for officers in game
		"blips_expire_after_seconds": 90, //The blips from above will be deleted after this number of seconds
		"enable_standalone_bolo_system": true //Enable this setting to use the built in BOLO system through `/addplate` and `/delplate` (NOTE: this setting must be set to false to use the SonoranCAD BOLOs below)
	},
	"integration": {
		"SonoranCAD_integration": { //All of these will require the SonoranCAD Framework to be installed
			"use": true, //Should any of the below CAD integrations be used?
			"add_live_map_blips": true, //Add blips for cameras to the livemap on the CAD
			"enable_911_calls": true, //Create 911 calls in CAD when a vehicle with a BOLO or that was speeding was detected
			"enable_cad_bolos": true, //Whether to check for BOLOs through the SonoranCAD
			"nearest_postal_plugin": "nearest-postal" //The name of the nearest-postal script you use, if you don't use one you can ignore this and it won't be used
		},
		"SpeedLimit_Display_integration": false, //To use this integration you will have to modify your SpeedLimitDisplay script as is described in the following section
		"Discord_Webhook": {
			"enabled": false, //Set to true and configure the webhook_url field below to use the Discord Webhook Feature
			"webhook_url": ""
		}
	},
	"notifications": {
		"type": "native" //Select notification type, available options: native, pNotify, okokNotify, cadOnly
    //cadOnly will only send 911 calls to CAD, if this isn't configured you can also use that to disable notifications entirely.
	}
}
```
Do not directly copy the config found above, the comments included will not work in your resource.

## Camera Location Config
You have two options for placing new cameras:
1. You can use the command `/spawnnewcam [name]` to spawn a new camera and generate the relevant config data
	- You may need to modify some of the rotation values manually to get that perfect placement you are looking for
2. You can manually copy and paste an existing config and then modify the values to meet your needs for the new camera
### `camera.json` property explanation
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