---
title: DUI Message Protocol
published: true
date: 2026-07-27T00:00:00.000Z
tags: [dui, nui, message protocol]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Reference the implemented browser readiness callback and full display-state message.
---

# DUI Message Protocol

This page documents the implemented boundary between the Lua client and the bundled browser interface. It is intended for developers maintaining the resource.

The DUI URL is fixed:

```text
https://cfx-nui-sonoran-stationdisplay/web/index.html?dui=1&displayId=<id>
```

Display configuration cannot supply a URL.

## Browser to Lua: `duiReady`

The browser posts the NUI callback after loading:

```json
{
  "displayId": "ssd-12345678-abcdef"
}
```

| Property | Value |
| --- | --- |
| Direction | Browser → Lua NUI callback |
| Callback name | `duiReady` |
| Required field | `displayId` string |
| Optional fields | None |
| Sent | Once after the bundled app initializes |
| Response | `{"ok": true}` when the renderer exists, otherwise `{"ok": false}` |

Lua marks that renderer ready and sends a forced full state. A delayed initial send also occurs after 500 milliseconds for FiveM builds where the callback is unavailable or delayed.

## Lua to browser: `DISPLAY_STATE_UPDATE`

This is the only implemented Lua-to-DUI action:

```json
{
  "action": "DISPLAY_STATE_UPDATE",
  "payload": {
    "displayId": "ssd-12345678-abcdef",
    "displayName": "Mission Row Briefing",
    "page": "ACTIVE_UNITS",
    "serviceFilter": "LEO",
    "grouping": "SERVICE",
    "sorting": "CALLSIGN",
    "units": [],
    "emergencyCalls": [],
    "dispatchCalls": [],
    "map": {
      "mode": "AUTO_FIT",
      "center": null,
      "zoom": 1,
      "showCalls": true,
      "showStationMarker": true
    },
    "station": {"x": 425.0, "y": -980.0, "z": 30.0},
    "maximumUnitsPerPage": 12,
    "maximumCallsPerPage": 8,
    "visibleCallFields": {
      "priority": true,
      "status": true,
      "address": true,
      "postal": true,
      "description": true,
      "assignedUnits": true,
      "caller": false
    },
    "rotation": {
      "enabled": true,
      "paused": false,
      "interval": 30,
      "elapsed": 0,
      "progress": 0,
      "pages": ["ACTIVE_UNITS", "EMERGENCY_CALLS"],
      "index": 1
    },
    "connected": true,
    "revision": 1,
    "staleAfterSeconds": 30,
    "serverTime": 1785182400
  }
}
```

### Required state fields

| Field | Type | Purpose |
| --- | --- | --- |
| `displayId` | string | Renderer/display key |
| `displayName` | string | Header title |
| `page` | page ID | Page to render |
| `serviceFilter` | service filter | Header label and already-filtered state |
| `grouping` | grouping ID | Unit groups |
| `sorting` | sorting ID | Unit row ordering |
| `units` | array | Normalized visible units |
| `emergencyCalls` | array | Normalized visible emergency calls |
| `dispatchCalls` | array | Normalized visible dispatch calls |
| `map` | object | Mode, center, zoom, and marker toggles |
| `station` | vector | Display position used by the station marker |
| `maximumUnitsPerPage` | integer | Unit list budget |
| `maximumCallsPerPage` | integer | Call list budget |
| `visibleCallFields` | object | Call-card field switches |
| `rotation` | object | Progress and ordered page state |
| `connected` | boolean | CAD live/disconnected overlay |
| `revision` | integer | Shared client cache revision |

`staleAfterSeconds` and `serverTime` are included for freshness context but are not currently rendered as per-record badges.

### Unit shape

Important fields:

```json
{
  "id": "unit-id",
  "callsign": "1A-12",
  "name": "A. Rivera",
  "status": "AVAILABLE",
  "statusCode": 2,
  "service": "LEO",
  "agency": "Police Department",
  "subdivision": "Patrol",
  "location": "Mission Row",
  "postal": "125",
  "coordinates": {"x": 425.0, "y": -980.0, "z": 30.0},
  "assignedCallId": null,
  "panic": false,
  "updatedAt": 1785182400,
  "stale": false
}
```

### Call shape

Important fields:

```json
{
  "id": "C-1042",
  "kind": "DISPATCH_CALL",
  "title": "Traffic Collision",
  "code": "11-79",
  "priority": 2,
  "status": "ACTIVE",
  "address": "Vespucci Blvd",
  "postal": "134",
  "description": "Two vehicles blocking lanes.",
  "assignedUnitIds": ["unit-id"],
  "assignedUnits": ["1A-12"],
  "caller": null,
  "coordinates": {"x": 310.0, "y": -1120.0, "z": 0.0},
  "createdAt": 1785182300,
  "updatedAt": 1785182400,
  "service": "LEO"
}
```

## Send behavior

The Lua client serializes and sends state:

* After DUI readiness
* When filtered display/cache state changes
* On explicit refresh
* On page changes
* At most once per second while updating rotation progress state

Identical encoded state is not sent again unless forced. The browser animates the progress bar locally.

## No granular browser messages

Version 1.0.0 does not send `UNIT_ADDED`, `CALL_UPDATED`, `FULL_SYNC`, `SET_PAGE`, `PAUSE_ROTATION`, or similar browser actions. Lua may receive granular network cache deltas, but it builds and sends one `DISPLAY_STATE_UPDATE` message to the DUI.

## Administration messages

The bundled browser source contains dormant administration markup/message handlers, but the Lua resource does not send `ADMIN_OPEN`/`ADMIN_CLOSE` or register the related NUI callbacks in version 1.0.0. They are not supported public protocol.

The customer management interface is the self-contained WarMenu.

## Versioning

There is no explicit protocol version field in 1.0.0. Treat new required fields or action names as a resource-version change and update Lua and browser files together. Do not replace only `web/app.js` during an update.
