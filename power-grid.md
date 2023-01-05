---
title: Power Grid
description: 
published: true
date: 2023-01-05T20:48:09.991Z
tags: 
editor: markdown
dateCreated: 2022-05-11T21:57:17.341Z
---

# Power Grid
![power-grid.png](/power-system/power-grid.png)
https://youtu.be/3JhmcqlOWv8
## Features

-   Ability to operate standalone
-   Support for AcePerms, ESX, QBCore, and custom permissions systems
		![ps-minigame.png](/power-system/ps-minigame.png)
    ![notification-feature.png](/speed-camera/notification-feature.png)
		![ps-cad-integration.png](/power-system/ps-cad-integration.png)
    ![ps-in-game-blips.png](/power-system/ps-in-game-blips.png)
    ![auto-update-feature.png](/speed-camera/auto-update-feature.png)
    ![ps-gun-placement-system.png](/power-system/ps-gun-placement-system.png)
    ![ps-discord-webhooks.png](/power-system/ps-discord-webhooks.png)
    ![ps-highly-configurable.png](/power-system/ps-highly-configurable.png)
    ![translate-feature.png](/speed-camera/translate-feature.png)

## Commands

These are the default names of commands, they may have been modified by the server owner.
| Command Name | Command Description | Required Permission |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/spawnnewsystem [prop] [name]` | This command will allow an admin to spawn a new power system using a gun placement system where the name argument is the label for the new power system and the prop argument is the text name of the model to use | Admin or as configured |
| `/cancelsystemplacement` | This command will cancel the current power system placement if one is currently in progress. | N/A |
| `/showsystemids` | This command will draw the ID of power systems which you are close to with 3D text near the base of the power system | N/A |
| `/getpositiondata [system id]` | This command will print the current positional data of the power system to the chat | Admin or as configured |
| `/changesystemdata [system data] [position data type] [value]` | This command will change the value which you specify of the power system you specify, run this command without arguments for example usage. All changes made with this command will be immediately saved | Admin or as configured |
| `/reloadpowersystems` | This command will completely reload the systems.json from the server's storage | Admin or as configured |
| `/pslink [system id]` | This command will allow the admin to link a power system to a different system using a system similar to the gun placement system | Admin or as configured |
| `/cancelpslink` | This command will cancel the current link in progress | N/A |

## Model Options

![ps-model-options-feature.png](/power-system/ps-model-options-feature.png)
The model on the left is named `prop_street_light_solar_panel` and the model is named `prop_powerbox`.

## Changelog
### v1.0.7
#### Feature
- `Add color indicators to hacking mini game. Green numbers mean both the number entered is correct and in the correct position. Red numbers mean the number is either incorrect or not in the correct position`
### v1.0.3
- `Includes a possible fix for higher than normal server timings on player join for certain ESX configurations`
### v1.0.1
- `Fixed: discord.CHANGEME.lua file added to escrowignore (required config file for configuring discord webhooks)`
### v1.0.0
- `Initial Release`