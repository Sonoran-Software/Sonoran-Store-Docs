---
title: Configuration
published: true
date: 2026-07-27T00:00:00.000Z
tags: [configuration, display models, performance]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Reference every customer-editable Sonoran Station Displays setting, default, limit, and validation rule.
---

# Configuration

`sonoran-stationdisplay/config.lua` contains server-wide defaults, integration settings, security limits, and the display model registry. Physical displays save their individual settings in `data/displays.json`.

{% hint style="warning" %}
Stop the resource before editing `config.lua`. Existing displays retain their saved per-display values when a default changes.
{% endhint %}

## General and integration settings

| Setting | Type | Default / allowed | Purpose and validation | Restart / performance |
| --- | --- | --- | --- | --- |
| `Config.configuration_version` | number | `1.0` | Configuration schema marker. Do not change independently. | Resource restart; no runtime cost. |
| `Config.Debug` | boolean | `false` | Enables debug logging and includes cached unit identifiers in normalized debug data. | Resource restart recommended; may expose more diagnostic detail and logs. |
| `Config.Locale` | string | `"en"` | Selects `Locales.<value>`. Only English ships by default. | Resource restart; invalid locale causes missing message lookups. |
| `Config.AdminCommand` | string | `"stationdisplay"` | Chat command that opens the management menu. Do not include `/`. | Resource restart to re-register the command. |
| `Config.SonoranCADResource` | string | `"sonorancad"` | Exact FiveM resource name used for state checks, metadata, and exports. | Resource restart; must match `ensure` name. |
| `Config.RequireSonoranCADVersion` | semver string | `"4.0.72"` | Minimum version accepted by the adapter. A lower or unparseable version is incompatible. | Resource restart; do not lower without testing the required exports. |
| `Config.VersionCheck.enabled` | boolean | `true` | Enables remote update checks in packaged builds. | Resource restart; one HTTP request per interval. |
| `Config.VersionCheck.url` | string | `"$VERSION_URL"` in source | Packaging replaces the token with the Sonoran version endpoint. A checkout containing the token skips checks safely. | Resource restart; use the official package value. |
| `Config.VersionCheck.intervalHours` | number | `6`; effective minimum `1` | Hours between remote version checks. | Resource restart; negligible network cost. |

## New-display defaults

| Setting | Type | Default / allowed | Purpose and validation | Restart / performance |
| --- | --- | --- | --- | --- |
| `Config.DefaultRotationInterval` | seconds | `30` | Default automatic page interval for new or invalid displays. | Resource restart; existing displays do not change. Rotation does not change CAD freshness. |
| `Config.MinimumRotationInterval` | seconds | `5` | Lowest accepted per-display interval. | Resource restart; lower values increase browser page changes. |
| `Config.MaximumRotationInterval` | seconds | `300` | Highest accepted per-display interval. | Resource restart. |
| `Config.DefaultServiceFilter` | enum string | `"BOTH"`; `LEO`, `FIRE_EMS`, `BOTH` | Initial filter and fallback for invalid saved values. | Resource restart; existing valid values persist. |
| `Config.DefaultGrouping` | enum string | `"SERVICE"`; `SERVICE`, `AGENCY`, `SUBDIVISION`, `NONE` | Initial Active Units grouping. | Resource restart; local browser work only. |
| `Config.DefaultSorting` | enum string | `"CALLSIGN"`; `CALLSIGN`, `STATUS`, `AGENCY`, `SUBDIVISION` | Initial alphabetical/natural unit sorting field. | Resource restart; local browser work only. |
| `Config.DefaultPage` | page ID | `"ACTIVE_UNITS"` | New-display default and fallback when no valid pages remain. Must be a supported page ID. | Resource restart. |
| `Config.DefaultPages` | ordered array | `{"ACTIVE_UNITS", "EMERGENCY_CALLS"}` | Initial enabled pages and rotation order. Invalid IDs and duplicates are removed during validation. | Resource restart; more pages do not create more DUIs. |
| `Config.MaximumUnitsPerPage` | integer | `12`; saved per-display range `4–40` | Default unit row budget. With multiple groups, the renderer divides the budget across groups. | Resource restart; higher values increase browser layout work. |
| `Config.MaximumCallsPerPage` | integer | `8`; saved per-display range `2–30` | Default call-card budget. | Resource restart; higher values increase browser layout work. |

Supported page IDs:

```text
ACTIVE_UNITS
EMERGENCY_CALLS
DISPATCH_CALLS
LIVE_MAP
```

## Cache and freshness settings

| Setting | Type | Default | Purpose and validation | Restart / performance |
| --- | --- | --- | --- | --- |
| `Config.UnitRefreshInterval` | milliseconds | `2000` | Central server poll interval for unit cache and coordinates. | Resource restart. Lower values increase SonoranCADFiveM export calls and client deltas. |
| `Config.CallRefreshInterval` | milliseconds | `3000` | Central server poll interval for dispatch and emergency caches. | Resource restart. Lower values increase export calls. |
| `Config.MapRefreshInterval` | milliseconds | `1000` | Present in configuration, but version 1.0.0 does not read this value. Map updates follow cache deltas and the renderer's state-send loop. | Changing it has no runtime effect in version 1.0.0. |
| `Config.StaleAfterSeconds` | seconds | `30` | Marks an unchanged normalized unit stale internally. | Resource restart. The shipped UI does not show a dedicated stale badge. |

Push events can request refreshes before the next poll. These intervals are shared server-wide and are not multiplied by display count.

## Distance, DUI, and capacity settings

| Setting | Type | Default / allowed | Purpose and validation | Restart / performance |
| --- | --- | --- | --- | --- |
| `Config.RenderDistance` | meters | `75.0`; per-display range `5–250` | Default distance at which a client creates the object and DUI. | Resource restart for new displays. Larger values increase simultaneous objects/DUIs. |
| `Config.InteractionDistance` | meters | `3.0`; saved range `0.5–15` | Saved default interaction distance. The bundled next/previous commands use the global value. | Resource restart. |
| `Config.MenuNearbyDistance` | meters | `15.0` | Radius used by **View Nearby Displays**. | Resource restart. Keep at or below maximum placement distance for edit/delete proximity checks. |
| `Config.DuiWidth` | pixels | `1280` | Browser texture width. | Resource restart. Higher values increase GPU/browser memory. |
| `Config.DuiHeight` | pixels | `720` | Browser texture height. | Resource restart. Default interface is 16:9. |
| `Config.DuiLoadTimeout` | milliseconds | `10000` | Reserved configuration value; version 1.0.0 does not apply a readiness timeout from this setting. | Changing it has no runtime effect in version 1.0.0. |
| `Config.DuiDestroyDelay` | milliseconds | `3000` | Delay before destroying an out-of-range display to reduce boundary thrashing. | Resource restart. Larger values retain DUIs longer. |
| `Config.MaximumActiveDuis` | integer | `8` | Hard per-client pool limit. New nearby renderers wait until capacity becomes available. | Resource restart. Primary browser-memory safeguard. |
| `Config.MaximumDisplays` | integer | `200` | Server limit for persisted displays. New saves are denied at the limit. | Resource restart; does not mean 200 DUIs may be active on one client. |
| `Config.MaximumDisplayNameLength` | integer | `64` | Maximum trimmed display name. Empty or longer names fail validation. | Resource restart. |
| `Config.MaximumRenderDistance` | meters | `250.0` | Maximum accepted saved render distance. | Resource restart. |
| `Config.MaximumPlacementDistance` | meters | `15.0` | Server proximity limit for saves and deletes and client placement confirmation. | Resource restart; security-sensitive. |
| `Config.EventRateLimit.windowMs` | milliseconds | `10000` | Per-player, per-action rate-limit window. | Resource restart. |
| `Config.EventRateLimit.maxRequests` | integer | `25` | Requests allowed in each action bucket per window. Excess requests are ignored. | Resource restart; security-sensitive. |

## Permission settings

| Setting | Type | Default / allowed | Purpose and validation | Restart |
| --- | --- | --- | --- | --- |
| `Config.PermissionMode` | string | `"ace"`; `ace` or `custom` | Selects the server permission provider. Any value other than `custom` uses ACE behavior. | Resource restart. |
| `Config.AcePermissions.view` | string | `"sonoran.display.view"` | View/menu/data permission object. Empty values deny. | Resource restart recommended. |
| `Config.AcePermissions.place` | string | `"sonoran.display.place"` | Create permission object. | Resource restart recommended. |
| `Config.AcePermissions.edit` | string | `"sonoran.display.edit"` | Edit/move/refresh permission object. | Resource restart recommended. |
| `Config.AcePermissions.delete` | string | `"sonoran.display.delete"` | Delete permission object. | Resource restart recommended. |
| `Config.AcePermissions.diagnostics` | string | `"sonoran.display.diagnostics"` | Diagnostics permission object. | Resource restart recommended. |
| `Config.CustomPermission` | function | Returns `false` | Called as `(source, action)` in custom mode. Only boolean `true` allows; errors deny. | Resource restart after editing. |

See [Permissions](permissions.md) for full examples.

## Call classification and visible fields

| Setting | Type | Default | Behavior |
| --- | --- | --- | --- |
| `Config.CallClassificationFallback.showUnclassifiedOnLeo` | boolean | `false` | Shows `OTHER` calls on LEO-only displays. |
| `Config.CallClassificationFallback.showUnclassifiedOnFireEms` | boolean | `false` | Shows `OTHER` calls on Fire/EMS-only displays. |
| `Config.VisibleCallFields.priority` | boolean | `true` | Shows the `P#` priority badge. |
| `Config.VisibleCallFields.status` | boolean | `true` | Included in state, but the version 1.0.0 call-card renderer does not display a status field. |
| `Config.VisibleCallFields.address` | boolean | `true` | Shows address and, when present, appends postal. |
| `Config.VisibleCallFields.postal` | boolean | `true` | Included in configuration, but postal display currently follows `address`; it is not independently toggled. |
| `Config.VisibleCallFields.description` | boolean | `true` | Shows a description block clipped to the card's two-line area. |
| `Config.VisibleCallFields.assignedUnits` | boolean | `true` | Shows assigned callsigns or `AWAITING ASSIGNMENT`. |
| `Config.VisibleCallFields.caller` | boolean | `false` | Controls normalization of emergency caller text, but the version 1.0.0 board renderer does not render caller data. Keep disabled. |

All changes require a resource restart. Caller information is hidden by default for privacy.

## Display model registry

`Config.DisplayModels` is the allowlist. Player requests cannot spawn a model outside it.

### Flat Screen TV

```lua
{
    model = "prop_tv_flat_01",
    label = "Flat Screen TV",
    renderMethod = "WORLD_PLANE",
    renderTarget = "tvscreen",
    screen = {
        center = {x = 0.0, y = -0.045, z = 0.02},
        width = 1.02,
        height = 0.57,
        flipX = false
    }
}
```

### Laptop Display

```lua
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
```

Model fields:

| Field | Type | Purpose |
| --- | --- | --- |
| `model` | string model name | Spawn model and validation key. A model hash is calculated client-side. |
| `label` | string | Menu label. |
| `renderMethod` | enum | `WORLD_PLANE`, `NAMED_RENDER_TARGET`, or `TEXTURE_REPLACE`. |
| `renderTarget` | string | GTA named render target used only by `NAMED_RENDER_TARGET`. |
| `textureDictionary` / `textureName` | string | Original texture pair used only by `TEXTURE_REPLACE`. |
| `screen.center` | vector | Local X/Y/Z offset for the `WORLD_PLANE` surface. |
| `screen.width` / `height` | number | World dimensions of the surface. Use a 16:9 proportion unless intentionally calibrating another aspect ratio. |
| `screen.flipX` | boolean | Reverses horizontal UVs when a surface appears mirrored. |

### Test a custom model

1. Confirm the model exists with `IsModelInCdimage`/`IsModelValid` in a development resource.
2. Add one registry entry and keep `renderMethod = "WORLD_PLANE"`.
3. Place it on a test server.
4. Adjust local center, width, height, rotation, and `flipX`.
5. Test both sides, multiple camera angles, several distances, and two identical instances with different pages.
6. Test resource stop/restart and out-of-range cleanup.

Not every television works automatically. Model geometry varies, named render-target names may not exist, and texture replacement is global to the model. An invalid model does not load. An invalid named target can remain black or draw nowhere. Use `WORLD_PLANE` for independent per-display content.

## Advanced service overrides

```lua
Config.AdvancedServiceOverridesEnabled = false
Config.AdvancedServiceOverrides = {}
```

When enabled, keys are matched case-insensitively against cached department, subdivision, or group text. Values must be:

```text
LEO
FIRE_EMS
OTHER
```

Example for a confirmed custom cache:

```lua
Config.AdvancedServiceOverridesEnabled = true
Config.AdvancedServiceOverrides = {
    ["STATE POLICE"] = "LEO",
    ["RESCUE"] = "FIRE_EMS"
}
```

Normal SonoranCADFiveM installations should leave this disabled.

## Map defaults

| Setting | Type | Default / allowed | Behavior |
| --- | --- | --- | --- |
| `Config.Map.defaultMode` | enum | `"AUTO_FIT"`; `AUTO_FIT`, `FIXED_REGION`, `FOLLOW_UNITS` | Initial map mode and invalid-value fallback. |
| `Config.Map.defaultCenter` | X/Y table | `{x = 215.0, y = -810.0}` | Initial fixed-region center. Saved center accepts `-20000` to `20000`. |
| `Config.Map.defaultZoom` | number | `1.0`; saved range `0.1–10.0` | Fixed-region zoom. |
| `Config.Map.minimumSpan` | number | `600.0` | Present in configuration, but version 1.0.0 uses a hard-coded 600-unit X minimum and does not read this value. |
| `Config.Map.padding` | number | `0.12` | Present in configuration, but version 1.0.0 uses a hard-coded fit multiplier and does not read this value. |
| `Config.Map.showCalls` | boolean | `true` | Initial call-marker toggle. |
| `Config.Map.showStationMarker` | boolean | `true` | Initial mounted-display marker toggle. |

Changes require a resource restart and affect new displays, not existing saved map values.

## Typical server example

This is a complete `config.lua` example using the shipped values. It does not create physical displays; use the in-game menu afterward.

```lua
Config = {}

Config.configuration_version = 1.0
Config.Debug = false
Config.Locale = "en"
Config.AdminCommand = "stationdisplay"

Config.SonoranCADResource = "sonorancad"
Config.RequireSonoranCADVersion = "4.0.72"
Config.VersionCheck = {
    enabled = true,
    url = "$VERSION_URL",
    intervalHours = 6
}

Config.DefaultRotationInterval = 30
Config.MinimumRotationInterval = 5
Config.MaximumRotationInterval = 300
Config.DefaultServiceFilter = "BOTH"
Config.DefaultGrouping = "SERVICE"
Config.DefaultSorting = "CALLSIGN"
Config.DefaultPage = "ACTIVE_UNITS"
Config.DefaultPages = {
    "ACTIVE_UNITS",
    "EMERGENCY_CALLS",
    "DISPATCH_CALLS",
    "LIVE_MAP"
}
Config.MaximumUnitsPerPage = 12
Config.MaximumCallsPerPage = 8

Config.UnitRefreshInterval = 2000
Config.CallRefreshInterval = 3000
Config.MapRefreshInterval = 1000
Config.StaleAfterSeconds = 30

Config.RenderDistance = 75.0
Config.InteractionDistance = 3.0
Config.MenuNearbyDistance = 15.0
Config.DuiWidth = 1280
Config.DuiHeight = 720
Config.DuiLoadTimeout = 10000
Config.DuiDestroyDelay = 3000
Config.MaximumActiveDuis = 8

Config.MaximumDisplays = 200
Config.MaximumDisplayNameLength = 64
Config.MaximumRenderDistance = 250.0
Config.MaximumPlacementDistance = 15.0
Config.EventRateLimit = {windowMs = 10000, maxRequests = 25}

Config.PermissionMode = "ace"
Config.AcePermissions = {
    view = "sonoran.display.view",
    place = "sonoran.display.place",
    edit = "sonoran.display.edit",
    delete = "sonoran.display.delete",
    diagnostics = "sonoran.display.diagnostics"
}
Config.CustomPermission = function(_, _)
    return false
end

Config.CallClassificationFallback = {
    showUnclassifiedOnLeo = false,
    showUnclassifiedOnFireEms = false
}

Config.VisibleCallFields = {
    priority = true,
    status = true,
    address = true,
    postal = true,
    description = true,
    assignedUnits = true,
    caller = false
}

Config.DisplayModels = {
    {
        model = "prop_tv_flat_01",
        label = "Flat Screen TV",
        renderMethod = "WORLD_PLANE",
        renderTarget = "tvscreen",
        screen = {
            center = {x = 0.0, y = -0.045, z = 0.02},
            width = 1.02,
            height = 0.57,
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

Config.AdvancedServiceOverridesEnabled = false
Config.AdvancedServiceOverrides = {}

Config.Map = {
    defaultMode = "AUTO_FIT",
    defaultCenter = {x = 215.0, y = -810.0},
    defaultZoom = 1.0,
    minimumSpan = 600.0,
    padding = 0.12,
    showCalls = true,
    showStationMarker = true
}
```

After restarting, create:

| Location | Filter | Pages | Rotation |
| --- | --- | --- | --- |
| Police briefing room | LEO | Active Units, Dispatch Calls, Live Map | On, 30 seconds |
| Fire station | Fire/EMS | Active Units, Emergency Calls, Dispatch Calls, Live Map | On, 30 seconds |
| Combined dispatch center | Both | All four pages | On, 30 seconds |

Use the ACE examples on [Permissions](permissions.md). These display profiles are saved through the menu, not by adding a nonexistent `Config.Displays` table.

