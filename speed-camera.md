---
title: Speed Cameras/ALPR
description: Automated system to detect and warn when a vehicle with an active BOLO or a speeding vehicle is spotted.
published: false
date: 2022-04-18T18:18:13.051Z
tags: 
editor: markdown
dateCreated: 2022-03-31T18:53:58.629Z
---

![1920x1080-speedcam.png](/speed-camera/1920x1080-speedcam.png)
# Speed Cameras/ALPR

## Features
- Ability to operate standalone
- Support for AcePerms, ESX, QBCore, and custom permissions systems
![cad-integration-feature.png](/speed-camera/cad-integration-feature.png)
![in-game-blip.png](/speed-camera/in-game-blip.png)
![discord-webhook-feature.png](/speed-camera/discord-webhook-feature.png)
![configurable-feature.png](/speed-camera/configurable-feature.png)

## Commands
| Command Name          | Command Description                                                                                                                         | Required Permission    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/spawnnewcam [prop] [name]` | This command will allow an admin to spawn a new camera using a gun placement system where the name argument is the label for the new camera and the prop argument is the text name of the model to use | Admin or as configured |
| `/addplate [plate]`           | This command will add a plate to the standalone BOLO system when in use.                                                                    | LEO or as configured   |
| `/delplate [plate]`           | This command will remove a plate from the standalone BOLO system when in use.                                                               | LEO or as configured   |
| `/cancelcamplacement` | This command will cancel the current camera placement if one is currently in progress.                                                      | N/A                    |

## Model Options
![promo-models.png](/speed-camera/promo-models.png)
The model on the left is named `prop_traffic_cam` and the model is named `radar01`.