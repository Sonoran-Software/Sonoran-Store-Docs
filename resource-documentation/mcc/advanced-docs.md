---
title: Mobile Command Center - Advanced Documentation
published: true
date: 2022-09-16T03:31:11.906Z
tags: null
editor: markdown
dateCreated: 2022-08-29T17:44:16.063Z
description: >-
  Developer Documentation and other Advanced Configuration Topics for the Mobile
  Command Center.
---

# Advanced Documentation

## Sonoran Radio Integration

The Sonoran `Mobile Command Center` automatically links its "Radio Repeaters" with Sonoran Radio. You must ensure that the Sonoran Radio resource is called `sonoranradio` **EXACTLY** or this integration **WILL NOT** work.

## Third Party Integration

#### Here is some useful information for potential third party integrations:

* Interior light is `Extra 4`

How to toggle menu:

```lua
WarMenu.OpenMenu('sonoranMccController')
```

### ALPR Documentation

#### Plate Read

When a plate is new **read** the event `Sonoran::mcc::plateRead` is fired to the **server**.

<table><thead><tr><th width="221">Parameter Name</th><th>Parameter Description</th></tr></thead><tbody><tr><td><code>plate</code></td><td>The plate currently read by the plate reader (<code>STRING</code>)</td></tr></tbody></table>

Example Usage Of Event:

```lua
RegisterNetEvent('Sonoran::mcc::plateRead', function(plate)
		print(plate)
end)
```

#### Plate Locked

When a plate is **locked** the event `Sonoran::mcc::plateLocked` is fired to the **server**.

<table><thead><tr><th width="222">Parameter Name</th><th>Parameter Description</th></tr></thead><tbody><tr><td><code>plate</code></td><td>The plate currently locked by the plate reader (<code>STRING</code>)</td></tr></tbody></table>

Example Usage Of Event:

```lua
RegisterNetEvent('Sonoran::mcc::plateLocked', function(plate)
		print(plate)
end)
```
