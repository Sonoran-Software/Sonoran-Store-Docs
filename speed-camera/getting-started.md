---
title: Getting Started
description: This page will walk you through getting and installing the script.
published: false
date: 2022-03-31T19:36:55.998Z
tags: 
editor: markdown
dateCreated: 2022-03-31T19:23:48.740Z
---

# Getting Started
## Acquire the Script
After purchasing the script through the sonoran store you may download the script through the keymaster account that purchased the script. Upon downloading extract the file to a safe place.

## Install the Script
Inside the script package will be a file called config.CHANGEME.json you should rename that to be config.json. You can then install the scrupt as you would any other resource for FiveM.

## Configuring the Script
Default config.json:
```json
{
	"configuration_version": 1.0,
  "script_version": 1.0,
  "debugMode" = false,
	"ace_perms": {
		"use_ace": false,
		"ace_object_place": "sonoran.trafficcam",
		"ace_object_notification": "sonoran.police"
	},
	"framework": {
		"framework_enabled": true,
		"framework_type": "qb-core",
		"police_job_names": ["police"],
		"allowed_to_place_groups": ["admin"]
	},
	"custom": {
		"use_custom": false,
		"check_perms_server_side": true,
		"custom_place_event": "community::IsAdmin",
		"custom_notify_event": "community::IsPolice"
	},
	"camera_settings": {
		"ignore_emergency": true,
		"only_ignore_on_els": true,
		"show_camera_blips_for_police": false,
		"unit_system": "mph",
		"time_between_flags": 1
	},
	"standalone_features": {
		"show_notification_blips_for_police": true,
		"blips_expire_after_seconds": 90,
		"enable_standalone_bolo_system": true
	},
	"integration": {
		"SonoranCAD_integration": {
			"use": true,
			"add_live_map_blips": true,
			"enable_911_calls": true,
			"enable_cad_bolos": true,
			"nearest_postal_plugin": "nearest-postal"
		},
		"SpeedLimit_Display_integration": true,
		"Discord_Webhook": {
			"enabled": false,
			"webhook_url": ""
		}
	},
	"notifications": {
		"type": "native"
	}
}
```