---
title: Power Grid
description: 
published: false
date: 2022-05-23T18:43:07.820Z
tags: 
editor: markdown
dateCreated: 2022-05-11T21:57:17.341Z
---

PROMO IMAGE HERE - AWAITING WYATT

# Power Grid

## Features

-   Ability to operate standalone
-   Support for AcePerms, ESX, QBCore, and custom permissions systems
    ![notification-feature.png](/speed-camera/notification-feature.png)
    ![cad-integration-feature.png](/speed-camera/cad-integration-feature.png)
    ![in-game-blip.png](/speed-camera/in-game-blip.png)
    ![auto-update-feature.png](/speed-camera/auto-update-feature.png)
    ![gun-placement-feature.png](/speed-camera/gun-placement-feature.png)
    ![discord-webhook-feature.png](/speed-camera/discord-webhook-feature.png)
    ![configurable-feature.png](/speed-camera/configurable-feature.png)
    ![translate-feature.png](/speed-camera/translate-feature.png)

## Commands

These are the default names of commands, they may have been modified by the server owner.
| Command Name | Command Description | Required Permission |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/spawnnewsystem [prop] [name]` | This command will allow an admin to spawn a new camera using a gun placement system where the name argument is the label for the new camera and the prop argument is the text name of the model to use | Admin or as configured |
| `/cancelsystemplacement` | This command will cancel the current camera placement if one is currently in progress. | N/A |
| `/showsystemids` | This command will draw the ID of cameras which you are in the radius of with 3D text near the camera | N/A |
| `/getpositiondata` | This command will print the current positional data of the camera to the chat | Admin or as configured |
| `/changesystemdata` | This command will change the value which you specify of the camera you specify, run this command without arguments for example usage. All changes made with this command will be immediately saved | Admin or as configured |
| `/reloadpowersystems` | This command will completely reload the cameras.json from the server's storage | Admin or as configured |
| `/pslink [system id]` | This command will allow the admin to link a power system to a different system using a system similar to the gun placement system | Admin or as configured |
| `/cancelpslink` | This command will enable the camera with the ID specified as an argument, this resume that camera's ability to flag vehicles | N/A |

## Model Options

![promo-models.png](/speed-camera/promo-models.png)
The model on the left is named `prop_traffic_cam` and the model is named `radar01`.

## Changelog

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
