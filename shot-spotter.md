---
title: Shot Spotter
description: Quickly be alerted when gunshots are detected around San Andreas
published: false
date: 2022-04-28T02:54:59.668Z
tags: 
editor: markdown
dateCreated: 2022-03-24T01:45:51.587Z
---

# Shot Spotter

## Features
- Ability to operate standalone
- SonoranCAD integration for 911 calls, and livemap blips
- Support for AcePerms, ESX, QBCore, and custom permissions systems
![ss-in-game-blips.png](/shot-spotter/ss-in-game-blips.png)
![ss-discord-webhooks.png](/shot-spotter/ss-discord-webhooks.png)
![ss-translate-feature.png](/shot-spotter/ss-translate-feature.png)
![ss-highly-configurable.png](/shot-spotter/ss-highly-configurable.png)

## Commands
| Command Name          | Command Description                                                                                                                         | Required Permission    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/shotspot` | This command will toggle the user's shot spotter status, either enabling or disabling shot spotter alerts and blips | LEO or as configured |
| `/showspotterid` | Show the ID above the shot spotters | Admin
| `/showspotterpos` | Show the position of the shot spotters | Admin
| `/changepositiondata` | Change the position data of the shot spotter | Admin
| `/reloadspotters` | Reload all spotters and positions | Admin
| `/spawnnewspotter` | Activate the placement gun | Admin