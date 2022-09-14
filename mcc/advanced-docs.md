---
title: Mobile Command Center - Advanced Documentation
description: Developer Documentation and other Advanced Configuration Topics for the Mobile Command Center.
published: false
date: 2022-09-14T22:47:14.735Z
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