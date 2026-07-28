---
title: In-game Clock and Weather
published: true
date: 2026-07-28T00:00:00.000Z
tags: [clock, weather, gta time]
editor: markdown
dateCreated: 2026-07-28T00:00:00.000Z
description: Configure the GTA/FiveM clock and estimated weather shown in the display header.
---

# In-game Clock and Weather

The header uses the local client's GTA/FiveM world clock, not the computer clock, server time, or Sonoran CAD time.

## Time

```lua
Config.TimeFormat = 12
Config.ShowClockSeconds = false
```

`TimeFormat` accepts `12` or `24`. Twelve-hour output converts midnight and noon to `12` and adds `AM` or `PM`. Twenty-four-hour output is zero-padded. `ShowClockSeconds` adds seconds when enabled.

The client checks the GTA clock at 250 or 500 millisecond intervals and sends an update only when the formatted value changes. Different clients can briefly show different values if another resource changes time locally rather than synchronizing it server-wide.

Administrative webhooks, persistence metadata, and audit logs use real-world UTC where documented. They do not reuse this in-game clock.

## Weather

```lua
Config.Weather = {
    enabled = true,
    temperatureUnit = "F",
    updateInterval = 2000
}
```

The weather label follows GTA's current weather type. Temperature is an estimate chosen for that game-weather condition; it is not real-world meteorology or a weather API. `temperatureUnit` accepts `F` or `C`.

Disable `Weather.enabled` to remove the weather indicator. Lower update intervals add client polling without making the underlying GTA weather more accurate.
