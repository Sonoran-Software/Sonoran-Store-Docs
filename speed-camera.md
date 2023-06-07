---
title: Speed Cameras/ALPR
description: Automated system to detect and warn when a vehicle with an active BOLO or a speeding vehicle is spotted.
published: true
date: 2023-06-07T23:57:19.570Z
tags: 
editor: markdown
dateCreated: 2022-03-31T18:53:58.629Z
---

![1920x1080-speedcam.png](/speed-camera/1920x1080-speedcam.png)

# Speed Cameras/ALPR

## Features

-   Ability to operate standalone
-   Support for AcePerms, ESX, QBCore, and custom permissions systems
    ![notification-feature.png](/speed-camera/notification-feature.png)
    ![cad-integration-feature.png](/speed-camera/cad-integration-feature.png)
    ![in-game-blip.png](/speed-camera/in-game-blip.png)
    ![auto-fine.png](/speed-camera/auto-fine.png)
    ![auto-update-feature.png](/speed-camera/auto-update-feature.png)
    ![gun-placement-feature.png](/speed-camera/gun-placement-feature.png)
    ![discord-webhook-feature.png](/speed-camera/discord-webhook-feature.png)
    ![configurable-feature.png](/speed-camera/configurable-feature.png)
    ![translate-feature.png](/speed-camera/translate-feature.png)

## Commands

These are the default names of commands, they may have been modified by the server owner.
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

## Changelog
### v2.1.7
#### Hotpatch
- `Fix issue of rotation data not being called in the initial spawning of the traffic cameras`
### v2.1.6
#### Hotpatch
- `Update ESX shared object call in regards to depreciation notice`

### v2.1.5
#### Hotpatch
- `Add restart handling to prevent crashing`

### v2.1.4
#### Feature
- `Added support for custom scripts for speed limit display`
### v2.1.3
#### Hotfix
- `Fix Ace permission issues`

### v2.1.2
#### Hotfix
- `Fix ESX depreciation notice`
- `Fix QBCore Auto Fines`

### v2.1.1
#### Hotfix
- `Fix fxmanifest not reading discord.lua`

### v2.0.0
- `ALPR-only Mode`
- `Configurable Auto Fine System`
- `Integration with an Upcoming Sonoran Release`

### v1.5.4

#### Bugfixes and Improvements
- `Fixes a QB-Core bug that was experienced on certain servers and led to physics crashes`

### v1.5.0

#### Features

-   `Added ability to list BOLO plates with the standalone system`
-   `Added the ability to edit and reload cameras in real time for admins`
-   `Added the ability to enable and disable specific cameras`
-   `Added a development tool to draw the IDs of cameras in the vicinity as 3D text`

#### Bugfixes and Improvements

-   `Added a warning when both standalone and CAD BOLO systems are enabled`
-   `Overhauled the BOLO permissions system to allow selecting specific job grades allowed to access it for framework servers, and allowed a separate ace object to be used`
