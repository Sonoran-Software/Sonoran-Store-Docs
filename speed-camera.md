---
title: Speed Cameras/ALPR
description: Automated system to detect and warn when a vehicle with an active BOLO or a speeding vehicle is spotted.
published: false
date: 2022-04-12T21:59:40.956Z
tags: 
editor: markdown
dateCreated: 2022-03-31T18:53:58.629Z
---

![promo banner.png](/speed-camera/promo-banner.png)
# Speed Cameras/ALPR

## Features
- Ability to operate standalone
- SonoranCAD integration for 911 calls, livemap blips, and Warrants/BOLOs
- Support for AcePerms, ESX, QBCore, and custom permissions systems
- Add blip at last detected location for police as well as bips for cameras
- Discord webhook to push to Discord when a BOLO or speeder is caught
- Customizable messages for the CAD, in-game notifications, and the Discord webhook
- Highly configurable

## Commands
| Command Name          | Command Description                                                                                                                         | Required Permission    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/spawnnewcam [prop] [name]` | This command will allow an admin to spawn a new camera using a gun placement system where the name argument is the label for the new camera and the prop argument is the text name of the model to use | Admin or as configured |
| `/addplate [plate]`           | This command will add a plate to the standalone BOLO system when in use.                                                                    | LEO or as configured   |
| `/delplate [plate]`           | This command will remove a plate from the standalone BOLO system when in use.                                                               | LEO or as configured   |
| `/cancelcamplacement` | This command will cancel the current camera placement if one is currently in progress.                                                      | N/A                    |

## Model Options
### `radar01`
![radar01-preview.png](/speed-camera/radar01-preview.png)
### `prop_traffic_camera`
PREVIEW COMING SOON!