---
title: Shot Spotter
description: Quickly be alerted when gunshots are detected around San Andreas
published: true
date: 2022-11-02T18:49:54.716Z
tags: 
editor: markdown
dateCreated: 2022-03-24T01:45:51.587Z
---

![ss-final.png](/ss-final.png)
# Shot Spotter

## Features
- Ability to operate standalone
- Support for AcePerms, ESX, QBCore, and custom permissions systems
![ss-cad-integration.png](/shot-spotter/ss-cad-integration.png)
![ss-in-game-blips.png](/shot-spotter/ss-in-game-blips.png)
![ss-gun-placement-system.png](/ss-gun-placement-system.png)
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
| `/spawnnewspotter` | Activate the placement gun [SEE HERE](https://docs.sonoran.store/en/gun-placement) | Admin
| `/cancelspotterplacement` | Cancels the placement gun | Admin
| `togglespotter` | Toggle the spotter with the specified ID | Admin

## Changelog
### v1.2.4
#### Hotpatch
- `Fixed QB Core & ESX permissions on "spawnnewspotter" command`
- `Change deafult values for webhooks to false`

### v1.2.2
#### Hotpatch
- `Fixed notifications not working on spotter alerts`

### v1.2.1 
#### Hotpatch
- `Fixed Livemap Icons`

### v1.2.0
#### Bugfixes, Improvements & Features
- `Disabling of Shot Spotters`
- `"Network Time" / "Latency"`
- `QB/ ESX Admin Permissions`
- `Standalone Option`
- `Livemap Radius`
- `Powergrid Integration`
- `Dispatcher Check`
- `Separate Webhook Options`
- `OkOk Notify/pNotify Integration`
- `Numerous Bug Fixes`

### v1.1.0

#### Bugfixes and Improvements
- `Fixes CAD livemap bugs, typos & more misc. bugs`

### v1.0.0

- `Inital Release`