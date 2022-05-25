---
title: Power Grid
description: 
published: false
date: 2022-05-25T23:55:39.349Z
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
		![ps-cad-integration.png](/power-system/ps-cad-integration.png)
    ![ps-in-game-blips.png](/power-system/ps-in-game-blips.png)
    ![auto-update-feature.png](/speed-camera/auto-update-feature.png)
    ![ps-gun-placement-system.png](/power-system/ps-gun-placement-system.png)
    ![ps-highly-configurable.png](/power-system/ps-highly-configurable.png)
    ![translate-feature.png](/speed-camera/translate-feature.png)

## Commands

These are the default names of commands, they may have been modified by the server owner.
| Command Name | Command Description | Required Permission |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/spawnnewsystem [prop] [name]` | This command will allow an admin to spawn a new power system using a gun placement system where the name argument is the label for the new power system and the prop argument is the text name of the model to use | Admin or as configured |
| `/cancelsystemplacement` | This command will cancel the current camera placement if one is currently in progress. | N/A |
| `/showsystemids` | This command will draw the ID of cameras which you are in the radius of with 3D text near the camera | N/A |
| `/getpositiondata [system id]` | This command will print the current positional data of the camera to the chat | Admin or as configured |
| `/changesystemdata [system data] [position data type] [value]` | This command will change the value which you specify of the camera you specify, run this command without arguments for example usage. All changes made with this command will be immediately saved | Admin or as configured |
| `/reloadpowersystems` | This command will completely reload the cameras.json from the server's storage | Admin or as configured |
| `/pslink [system id]` | This command will allow the admin to link a power system to a different system using a system similar to the gun placement system | Admin or as configured |
| `/cancelpslink` | This command will enable the camera with the ID specified as an argument, this resume that camera's ability to flag vehicles | N/A |

## Model Options

![promo-models.png](/speed-camera/promo-models.png)
The model on the left is named `prop_traffic_cam` and the model is named `radar01`.

## Changelog

### v1.0.0
`Initial Release`