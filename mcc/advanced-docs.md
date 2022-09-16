---
title: Mobile Command Center - Advanced Documentation
description: Developer Documentation and other Advanced Configuration Topics for the Mobile Command Center.
published: true
date: 2022-09-16T03:27:39.740Z
tags: 
editor: markdown
dateCreated: 2022-08-29T17:44:16.063Z
---

# Advanced Documentation	 
## Sonoran Radio Integration
The Sonoran `Mobile Command Center` automatically links its "Radio Repeaters" with Sonoran Radio. You must ensure that the Sonoran Radio resource is called `sonoranradio` **EXACTLY** or this integration __WILL NOT__ work.

## Third Party Integration
#### Here is some useful information for potential third party integrations:

- Interior light is `Extra 4`
---
How to toggle menu:
```lua
WarMenu.OpenMenu('sonoranMccController')
```

### ALPR Documentation

#### Plate Read
When a plate is new **read** the event `Sonoran::mcc::plateRead` is fired to the **server**.

Example Usage Of Event:
```lua
RegisterNetEvent('Sonoran::mcc::plateRead', function(plate)
		print(plate)
end)
```

#### Plate Locked
When a plate is **locked** the event `Sonoran::mcc::plateLocked` is fired to the **server**.

Example Usage Of Event:
```lua
RegisterNetEvent('Sonoran::mcc::plateLocked', function(plate)
		print(plate)
end)
```