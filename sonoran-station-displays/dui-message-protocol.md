---
title: DUI Message Protocol
published: true
date: 2026-07-28T00:00:00.000Z
tags: [dui, nui, message protocol]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Reference the implemented browser readiness, state, clock, weather, map, and bodycam DUI messages.
---

# DUI Message Protocol

This page documents the internal boundary between the Lua client and the bundled browser. It is useful to maintainers but is not a promise that third-party browser replacements will remain compatible across versions.

The DUI URL is fixed:

```text
https://cfx-nui-sonoran-stationdisplay/web/index.html?dui=1&displayId=<id>
```

Display configuration cannot provide a browser or stream URL.

## Browser to Lua

### `duiReady`

After initialization:

```json
{"displayId": "ssd-12345678-abcdef"}
```

Lua accepts it only for an existing renderer, marks the DUI ready, and forces the current state, game time, and weather. A renderer that does not become ready within the internal 10-second timeout is destroyed and retried after the internal delay.

### `bodycamFeedStatus`

The bodycam viewer can report:

```json
{"displayId": "ssd-...", "unitId": "unit-id", "status": "unavailable"}
```

The client acknowledges the callback and increments its diagnostic load-failure counter for `unavailable`. It does not trust the callback to change server runtime state.

## Lua to browser

### `DISPLAY_STATE_UPDATE`

This is the primary state message:

```json
{
  "action": "DISPLAY_STATE_UPDATE",
  "payload": {
    "displayId": "ssd-12345678-abcdef",
    "displayName": "Mission Row Briefing",
    "theme": "MODERN",
    "page": "ACTIVE_UNITS",
    "serviceFilter": "LEO",
    "grouping": "SERVICE",
    "sorting": "CALLSIGN",
    "units": [],
    "emergencyCalls": [],
    "dispatchCalls": [],
    "bodycams": [],
    "bodycamConfig": {
      "layout": "AUTO",
      "maximumFeeds": 4,
      "rotationInterval": 15
    },
    "bodycamIntegration": {
      "available": true,
      "enabled": true,
      "mode": "PEER_STREAM"
    },
    "bodycamPresentation": {
      "delayedAfterSeconds": 8,
      "unavailableAfterSeconds": 30,
      "loadFailures": 0
    },
    "map": {
      "mode": "AUTO_FIT",
      "center": {"x": 215, "y": -810},
      "zoom": 1,
      "showCalls": true,
      "showStationMarker": true
    },
    "mapPresentation": {
      "minimumSpan": 600,
      "maximumSpan": 24000,
      "padding": 0.12,
      "debug": false
    },
    "station": {"x": 425, "y": -980, "z": 30},
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

Lua sends a changed state after readiness, cache/display/bodycam changes, explicit refresh, and runtime page changes. It also checks rotation progress at most once per second. Identical encoded state is suppressed unless forced; the browser animates progress between messages.

Important unit fields include ID, callsign, name, status/status code, service, agency, subdivision, location/postal, coordinates, assigned call, panic, update time, and stale state.

Important call fields include ID, kind, title/code, priority/status, address/postal, description, assigned IDs/callsigns, optional emergency caller, coordinates, timestamps, and classified service.

A bodycam feed includes the joined unit metadata plus a nested runtime record with active/mode/peer/stream readiness, reason, activation/update values, and sequence. The peer ID is not an arbitrary URL.

### `GAME_TIME_UPDATE`

```json
{
  "action": "GAME_TIME_UPDATE",
  "payload": {
    "hours": 21,
    "minutes": 42,
    "seconds": 5,
    "format": 12,
    "showSeconds": false
  }
}
```

The client sends only when the formatted GTA/FiveM time changes, or when forced after readiness.

### `WEATHER_UPDATE`

```json
{
  "action": "WEATHER_UPDATE",
  "payload": {
    "weatherType": "CLEAR",
    "temperature": 78,
    "unit": "F"
  }
}
```

Weather values describe the GTA world and an estimated temperature.

### `MAP_PAN`

```json
{
  "action": "MAP_PAN",
  "payload": {"direction": "LEFT", "step": 0.2}
}
```

It is accepted only for an available local renderer currently on Live Map. The Lua side clamps the step to `0.05` through `0.5`. Reset is represented by the allowed reset direction used by the browser handler.

### `BODYCAM_NAVIGATE`

```json
{
  "action": "BODYCAM_NAVIGATE",
  "payload": {"direction": "NEXT"}
}
```

It is accepted only for an available local renderer currently on Bodycams and changes that DUI's bodycam feed page.

## Versioning

There is no explicit protocol-version field. Update Lua and all `web/` files together. Do not replace only `web/app.js` or copy browser files between resource releases.

The web source also contains dormant administration markup/handlers. The current Lua resource does not open that NUI or register its administrative callbacks; the supported customer interface is WarMenu.
