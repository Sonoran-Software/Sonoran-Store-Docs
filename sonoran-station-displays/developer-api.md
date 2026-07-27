---
title: Developer API
published: true
date: 2026-07-27T00:00:00.000Z
tags: [lua exports, events, developer api]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Use the supported Sonoran Station Displays client/server exports and observation events.
---

# Developer API

Resource name:

```text
sonoran-stationdisplay
```

## Server exports

Server exports are trusted resource-to-resource calls. They do not apply player ACE checks. Never proxy untrusted client input directly into them.

### `GetDisplays()`

```lua
local displays = exports["sonoran-stationdisplay"]:GetDisplays()
```

| Property | Value |
| --- | --- |
| Side | Server |
| Parameters | None |
| Returns | Array of copied display tables |
| Failure | Empty array when no displays exist |
| Permission | None; server resource call |

### `GetDisplay(displayId)`

```lua
local display = exports["sonoran-stationdisplay"]:GetDisplay("ssd-...")
```

| Property | Value |
| --- | --- |
| Side | Server |
| `displayId` | string |
| Returns | Copied display table or `nil` |
| Failure | `nil` for an unknown ID |
| Permission | None; server resource call |

### `RefreshDisplay(displayId)`

```lua
local ok = exports["sonoran-stationdisplay"]:RefreshDisplay("ssd-...")
```

| Property | Value |
| --- | --- |
| Side | Server |
| `displayId` | string |
| Returns | boolean |
| Failure | `false` for an unknown ID |
| Behavior | Broadcasts a forced DUI state resend to authorized clients; does not refresh CAD caches |
| Permission | None; server resource call |

### `SetDisplayPage(displayId, pageId)`

```lua
local ok = exports["sonoran-stationdisplay"]:SetDisplayPage(
    "ssd-...",
    "LIVE_MAP"
)
```

| Property | Value |
| --- | --- |
| Side | Server |
| `displayId` | string |
| `pageId` | `ACTIVE_UNITS`, `EMERGENCY_CALLS`, `DISPATCH_CALLS`, or `LIVE_MAP` |
| Returns | boolean |
| Failure | `false` for an unknown display or page ID |
| Behavior | Broadcasts a runtime page request; does not persist current page |
| Permission | None; server resource call |

The receiving client applies the page only when it is in that display's saved `pageOrder`.

### `SetDisplayServiceFilter(displayId, serviceFilter)`

```lua
local ok = exports["sonoran-stationdisplay"]:SetDisplayServiceFilter(
    "ssd-...",
    "FIRE_EMS"
)
```

| Property | Value |
| --- | --- |
| Side | Server |
| `displayId` | string |
| `serviceFilter` | `LEO`, `FIRE_EMS`, or `BOTH` |
| Returns | boolean |
| Failure | `false` for an unknown display or filter |
| Behavior | Persists the filter and broadcasts the updated display |
| Permission | None; server resource call |

## Client exports

Client exports act on the local client's synchronized state. A player without `view` does not receive display/cache synchronization.

### `GetDisplays()`

```lua
local displays = exports["sonoran-stationdisplay"]:GetDisplays()
```

Returns an array sorted by display name. Treat returned display tables as read-only.

### `GetDisplay(displayId)`

```lua
local display = exports["sonoran-stationdisplay"]:GetDisplay("ssd-...")
```

Returns the local display table or `nil`. Treat it as read-only.

### `RefreshDisplay(displayId)`

```lua
exports["sonoran-stationdisplay"]:RefreshDisplay("ssd-...")
```

Returns no value. If that display has a local active DUI, its state is resent. It does not request server or CAD data.

### `SetDisplayPage(displayId, pageId)`

```lua
local ok = exports["sonoran-stationdisplay"]:SetDisplayPage(
    "ssd-...",
    "ACTIVE_UNITS"
)
```

Returns `true` when an active local renderer exists and the page is in its saved order; otherwise `false`. The change is local and not persisted.

### `OpenDisplayMenu()`

```lua
local requested = exports["sonoran-stationdisplay"]:OpenDisplayMenu()
```

Returns `false` when placement is already active; otherwise `true` after requesting a fresh server permission snapshot. Menu opening is asynchronous and still requires `view`.

## Supported client observation events

External client resources may listen to these events. Do not trigger the synchronization events to impersonate server state.

### Display configuration lifecycle

```lua
AddEventHandler("sonoranDisplays:client:displayAdded", function(display)
    print(("Added display %s"):format(display.id))
end)

AddEventHandler("sonoranDisplays:client:displayUpdated", function(display)
    print(("Updated display %s"):format(display.id))
end)

AddEventHandler("sonoranDisplays:client:displayRemoved", function(displayId)
    print(("Removed display %s"):format(displayId))
end)
```

| Event | Direction | Payload | Fires |
| --- | --- | --- | --- |
| `sonoranDisplays:client:displayAdded` | Server → authorized client | display table | After a persisted create |
| `sonoranDisplays:client:displayUpdated` | Server → authorized client | display table | After a persisted update |
| `sonoranDisplays:client:displayRemoved` | Server → authorized client | display ID string | After a persisted delete |

These are safe to listen to. External resources should mutate displays through supported server exports, not trigger these client events.

### Local object lifecycle

```lua
AddEventHandler("sonoranDisplays:client:displaySpawned", function(displayId, object)
    -- The local object entered range.
end)

AddEventHandler("sonoranDisplays:client:displayDespawned", function(displayId)
    -- The local object and DUI were cleaned up.
end)
```

| Event | Direction | Payload | Fires |
| --- | --- | --- | --- |
| `sonoranDisplays:client:displaySpawned` | Local client | display ID, object handle | After an in-range object is created |
| `sonoranDisplays:client:displayDespawned` | Local client | display ID | After local cleanup |

Object handles are client-local and can become invalid after the event.

### Open the menu

```lua
TriggerEvent("sonoranDisplays:client:openMenu")
```

This local event is safe for another client resource to trigger. It performs the same server permission handshake as the command/export.

### Runtime page broadcast

```lua
AddEventHandler("sonoranDisplays:client:setPage", function(displayId, pageId)
    -- Observe a server-authorized runtime page broadcast.
end)
```

| Property | Value |
| --- | --- |
| Direction | Server → authorized client |
| Payload | Display ID string, page ID string |
| Fires | After the server `SetDisplayPage` export broadcasts a valid request |
| Safe external use | Safe to listen to; use the server export rather than triggering this event directly |

The receiving renderer still requires the page to exist in the display's saved `pageOrder`.

## Display table

Persisted display fields include:

```text
id, name, model, position, rotation, serviceFilter,
enabledPages, pageOrder, defaultPage, rotationEnabled,
rotationInterval, grouping, sorting, hideUnavailable,
maxUnitsPerPage, maxCallsPerPage, map, renderDistance,
interactionDistance, routingBucket, interiorId, createdBy,
createdAt, updatedAt
```

## Internal network events

The resource also registers internal synchronization/request events such as:

```text
sonoranDisplays:server:requestSync
sonoranDisplays:server:requestMenu
sonoranDisplays:server:save
sonoranDisplays:server:delete
sonoranDisplays:server:forceRefresh
sonoranDisplays:server:requestDiagnostics
sonoranDisplays:client:displayFullSync
sonoranDisplays:client:cacheFullSync
sonoranDisplays:client:cacheDelta
sonoranDisplays:client:connection
sonoranDisplays:client:permissions
sonoranDisplays:client:openAuthorizedMenu
sonoranDisplays:client:diagnostics
sonoranDisplays:client:setPage
sonoranDisplays:client:forceRefresh
```

These are implementation protocol, not a stable public API. Player requests are permission checked, proximity checked where applicable, validated, and rate limited. External resources should not trigger them.

See [DUI Message Protocol](dui-message-protocol.md) for the browser boundary.
