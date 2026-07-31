---
title: Configuration
published: true
date: 2026-07-28T00:00:00.000Z
tags: [configuration, display models, performance]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Reference every customer-editable Station Displays setting, default, limit, and current implementation note.
---

# Configuration

Customer settings live in `sonoran-stationdisplay/config.lua`. Physical display records are saved separately in `data/displays.json`.

{% hint style="warning" %}
Stop the resource before editing configuration. A changed default does not overwrite an existing display's saved value.
{% endhint %}

The dependency name, required base CAD version, DUI resolution/lifecycle, event rate limit, and version-check schedule are internal. Do not copy old settings for those values into a current configuration.

## General

| Setting | Default | Behavior |
| --- | --- | --- |
| `Config.configuration_version` | `1.0` | Customer configuration schema marker. Do not change independently. |
| `Config.Debug` | `false` | Enables extra logs and the permission diagnostic command. May expose more identifiers/details. |
| `Config.Locale` | `"en"` | Selects `Locales.<value>`; English is the shipped locale. |
| `Config.AdminCommand` | `"stationdisplay"` | Management command without `/`. Restart to re-register it. |

### Promotional sample data

`Config.PromoMode` is a maintainer screenshot fixture:

```lua
Config.PromoMode = {
    enabled = false,
    animate = true,
    activityIntervalSeconds = 45
}
```

Keep it disabled on production servers. When present in a source/release-candidate build, the server console can use `stationdisplay_promodata on|off|status`. It produces fictional data and must never be presented as live CAD state.

## New-display and page defaults

| Setting | Default / allowed | Behavior |
| --- | --- | --- |
| `DefaultRotationInterval` | `30` seconds | Main page rotation for new displays. |
| `MinimumRotationInterval` | `5` seconds | Lowest accepted saved main-page interval. |
| `MaximumRotationInterval` | `300` seconds | Highest accepted saved main-page interval. |
| `DefaultServiceFilter` | `"BOTH"` | `LEO`, `FIRE_EMS`, or `BOTH`. |
| `DefaultGrouping` | `"SERVICE"` | `SERVICE`, `AGENCY`, `SUBDIVISION`, or `NONE`. |
| `DefaultSorting` | `"CALLSIGN"` | `CALLSIGN`, `STATUS`, `AGENCY`, or `SUBDIVISION`. |
| `DefaultTheme` | `"MODERN"` | `MODERN` or `RETRO`. |
| `DefaultPage` | `"ACTIVE_UNITS"` | Must be in the enabled page list. |
| `DefaultPages` | `{"ACTIVE_UNITS", "EMERGENCY_CALLS"}` | Ordered initial page list. Invalid IDs and duplicates are removed. |
| `MaximumUnitsPerPage` | `12` | Per-display saved range `4` to `40`; divided across visible groups. |
| `MaximumCallsPerPage` | `8` | Per-display saved range `2` to `30`. |

Supported page IDs:

```text
ACTIVE_UNITS
EMERGENCY_CALLS
DISPATCH_CALLS
LIVE_MAP
BODYCAMS
```

## Clock and weather

| Setting | Default / allowed | Behavior |
| --- | --- | --- |
| `TimeFormat` | `12`; `12` or `24` | GTA/FiveM world-clock format. |
| `ShowClockSeconds` | `false` | Adds seconds to the header clock. |
| `Weather.enabled` | `true` | Shows the current GTA weather label and estimated temperature. |
| `Weather.temperatureUnit` | `"F"`; `F` or `C` | Display unit for the estimate. |
| `Weather.updateInterval` | `2000` ms | Client weather sampling interval. |

See [In-game Clock and Weather](in-game-clock.md).

## Cache and freshness

| Setting | Default | Behavior / cost |
| --- | ---: | --- |
| `UnitRefreshInterval` | `2000` ms | Shared server poll for unit state and coordinates. Lower values increase CAD export calls and client deltas. |
| `CallRefreshInterval` | `3000` ms | Shared server poll for both call caches. |
| `MapRefreshInterval` | `1000` ms | Retained configuration value; the current implementation does not read it. |
| `StaleAfterSeconds` | `30` seconds | Marks unchanged units stale internally. Map markers reflect it; table rows have no separate badge. |

Push events can request an earlier refresh. Polling is server-wide and is not multiplied by display count.

## Bodycams

```lua
Config.Bodycams = {
    defaultLayout = "AUTO",
    defaultMaximumFeeds = 4,
    defaultRotationInterval = 15,
    minimumRotationInterval = 5,
    maximumRotationInterval = 300,
    maximumFeeds = 4,
    refreshIntervalMs = 1000,
    delayedAfterSeconds = 8,
    unavailableAfterSeconds = 30,
    showInactiveUnits = false,
    showUnitName = true,
    showAgency = true,
    showStatus = true,
    showAssignedCall = true,
    showTimestamp = true
}
```

| Setting | Behavior |
| --- | --- |
| `defaultLayout` | `AUTO`, `SINGLE`, `GRID`, or `FOCUSED` for new displays. |
| `defaultMaximumFeeds` | Initial feed count per bodycam page. |
| `defaultRotationInterval` | Independent bodycam-page interval, default 15 seconds. |
| `minimumRotationInterval` / `maximumRotationInterval` | Accepted saved range, 5 to 300 seconds. |
| `maximumFeeds` | Hard per-page ceiling; current implementation caps it at 4. |
| `refreshIntervalMs` | Server bodycam-runtime polling fallback. Runtime events can update sooner. |
| `delayedAfterSeconds` | A late periodic frame becomes `DELAYED`. |
| `unavailableAfterSeconds` | A longer capture gap becomes `CAPTURE FAILED` or `OFFLINE`. |
| `showInactiveUnits` | Includes associated inactive/off bodycams. |
| `showUnitName`, `showAgency`, `showStatus`, `showAssignedCall` | Toggle tile metadata. |
| `showTimestamp` | Controls capture-freshness/timestamp presentation when supplied. |

These controls do not change the internal 4-second local capture interval, JPEG quality,
300 KB frame ceiling, four-feed subscription limit, or recording behavior. See
[Bodycams](bodycams.md).

## Distance and capacity

| Setting | Default / range | Behavior |
| --- | --- | --- |
| `RenderDistance` | `75.0` m; saved `5` to `250` | New-display distance for object/DUI creation. |
| `InteractionDistance` | `2.5` m; saved `0.5` to `15` | Fallback targeting distance for page, map, and bodycam controls. |
| `MenuNearbyDistance` | `15.0` m | Radius for **View Nearby Displays**. |
| `MaximumActiveDuis` | `8` | Per-client active renderer pool limit. |
| `MaximumDisplays` | `200` | Server persistence limit. |
| `MaximumDisplayNameLength` | `64` | Maximum trimmed name length. |
| `MaximumRenderDistance` | `250.0` m | Highest saved render distance. |
| `MaximumPlacementDistance` | `15.0` m | Server proximity limit for create, move, edit, and delete. |

Internally, the client creates 1280 by 720 DUIs, waits up to 10 seconds for readiness, and delays destruction for 3 seconds so a quick range transition can be cancelled. The protected server allows 25 events per player/category per 10-second window.

## Permissions

```lua
Config.PermissionMode = "ace"
Config.AcePermissions = {
    menu = "sonoran.stationdisplay.menu",
    place = "sonoran.stationdisplay.place",
    edit = "sonoran.stationdisplay.edit",
    delete = "sonoran.stationdisplay.delete",
    diagnostics = "sonoran.stationdisplay.diagnostics",
    refresh = "sonoran.stationdisplay.refresh",
    webhookTest = "sonoran.stationdisplay.webhook.test"
}
```

`PermissionMode` accepts `ace` or `custom`. Empty ACE values deny. Viewing has no ACE
node; menu access, placement, editing, deletion, diagnostics, refresh, and webhook tests
are independently configurable.

`Config.CustomPermission(source, action)` is called only in custom mode. It must return
boolean `true`; errors deny. The callback receives backend actions `menu`, `place`,
`edit`, `delete`, `diagnostics`, `refresh`, and `webhookTest`. See
[Permissions](permissions.md).

## Call classification and fields

```lua
Config.CallClassificationFallback = {
    showUnclassifiedOnLeo = false,
    showUnclassifiedOnFireEms = false
}
```

These options allow `OTHER` calls on a single-service display. Both-service displays already show them.

| `VisibleCallFields` key | Default | Current rendering |
| --- | --- | --- |
| `priority` | `true` | Priority badge |
| `status` | `true` | Normalized but not rendered |
| `address` | `true` | Address and, when present, postal |
| `postal` | `true` | Not independent; postal currently follows `address` |
| `description` | `true` | Two-line clipped description |
| `assignedUnits` | `true` | Callsigns or awaiting-assignment label |
| `caller` | `false` | Emergency caller may be normalized when enabled, but is not rendered |

Keep caller disabled unless a future renderer and the display audience justify it.

## Display model registry

`Config.DisplayModels` is the server allowlist. Player requests cannot spawn another model.

```lua
Config.DisplayModels = {
    {
        model = "prop_tv_flat_01",
        label = "Flat Screen TV",
        renderMethod = "WORLD_PLANE",
        renderTarget = "tvscreen",
        screen = {
            center = {x = 0.0, y = -0.061, z = 0.48},
            width = 2.14,
            height = 1.20,
            flipX = false
        }
    },
    {
        model = "prop_laptop_jimmy",
        label = "Laptop Display",
        renderMethod = "WORLD_PLANE",
        textureDictionary = "prop_laptop_jimmy",
        textureName = "prop_jimmy_screen",
        screen = {
            center = {x = 0.0, y = -0.205, z = 0.22},
            width = 0.36,
            height = 0.205,
            flipX = false
        }
    }
}
```

| Field | Purpose |
| --- | --- |
| `model`, `label` | GTA model name and menu label |
| `renderMethod` | `WORLD_PLANE`, `NAMED_RENDER_TARGET`, or `TEXTURE_REPLACE` |
| `renderTarget` | Named target for that method |
| `textureDictionary`, `textureName` | Original texture pair for replacement |
| `screen.center` | Model-local X/Y/Z surface center |
| `screen.width`, `screen.height` | World dimensions; normally calibrate to 16:9 |
| `screen.flipX` | Corrects a mirrored horizontal surface |

Use `WORLD_PLANE` for independent display content. Named targets and texture replacement are global to identical model instances. Test custom models in-game from several angles, with two instances, and across restart/range cleanup.

## Advanced service overrides

```lua
Config.AdvancedServiceOverridesEnabled = false
Config.AdvancedServiceOverrides = {}
```

When enabled, case-insensitive department, subdivision, or group text maps to `LEO`, `FIRE_EMS`, or `OTHER`. Leave this disabled unless a confirmed customized/legacy cache has incorrect `unit.data.page` classification.

## Map defaults

```lua
Config.Map = {
    defaultMode = "AUTO_FIT",
    defaultCenter = {x = 215.0, y = -810.0},
    defaultZoom = 1.0,
    minimumSpan = 600.0,
    maximumSpan = 24000.0,
    padding = 0.12,
    showCalls = true,
    showStationMarker = true,
    debug = false
}
```

Modes are `AUTO_FIT`, `FIXED_REGION`, and `FOLLOW_UNITS`. Saved center components accept `-20000` to `20000`; zoom accepts `0.1` to `10.0`. Minimum/maximum span and padding constrain automatic fits. Debug adds a projection overlay and should remain off in production.

## Viewer interaction

```lua
Config.PageInteraction = {
    enabled = true,
    cooldown = 500,
    allowPrevious = true,
    pauseRotationAfterInteraction = 10,
    maximumViewAngle = 65.0,
    requireLineOfSight = true,
    nextKey = "RIGHT",
    previousKey = "LEFT"
}
```

Page changes are server-validated and synchronized to clients. The rotation timer holds
for 10 seconds by default after an interaction. A candidate display must be within its
interaction distance and camera angle, in the correct world context, and visible by
line of sight when required.

```lua
Config.MapInteraction = {
    enabled = true,
    cooldown = 150,
    panStep = 0.20,
    upKey = "NUMPAD8",
    downKey = "NUMPAD2",
    leftKey = "NUMPAD4",
    rightKey = "NUMPAD6",
    resetKey = "NUMPAD5"
}

Config.BodycamInteraction = {
    enabled = true,
    nextKey = "PAGEDOWN",
    previousKey = "PAGEUP"
}
```

Map pan is temporary and synchronized. Bodycam page navigation is local to the targeted DUI. All keys are registered mappings and can be rebound in FiveM settings.

## Restart and persistence notes

Global configuration changes require a resource restart unless a page explicitly states otherwise. Defaults affect new displays and invalid-value fallbacks. Existing valid per-display values remain in `data/displays.json`; edit them in the management menu if they should change.
