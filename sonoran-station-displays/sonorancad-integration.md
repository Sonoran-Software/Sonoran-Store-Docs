---
title: SonoranCADFiveM Integration
published: true
date: 2026-07-28T00:00:00.000Z
tags: [sonoran cad, cache, integration]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Understand how Sonoran Station Displays reads and classifies SonoranCADFiveM unit and call caches.
---

# SonoranCADFiveM Integration

## Supported interface

| Item | Value |
| --- | --- |
| Required resource name | `sonorancad` |
| Minimum version | `4.0.72` |
| Authentication | Uses local exports; no additional API key |

The server adapter calls:

```lua
exports["sonorancad"]:GetUnitCache()
exports["sonorancad"]:GetUnitByPlayerId(source)
exports["sonorancad"]:GetCallCache()
exports["sonorancad"]:GetEmergencyCache()
```

`GetUnitByPlayerId` associates a cached CAD unit with a connected server player when possible. Player connection alone does not create an on-duty unit.

The optional Bodycams page also calls `GetBodycamRuntime()` and observes
`SonoranCAD::bodycam::RuntimeChanged`. This bridge supplies authoritative active state
and player/unit ownership. Station Displays obtains imagery locally through
`screenshot-basic`; the bridge does not supply a peer, TURN configuration, relay URL,
or hosted image.

## Unit classification

The on-duty cache is the source of truth. Status code `100` (`OFFLINE`) is excluded defensively.

| `unit.data.page` | CAD service | Internal value | Display label |
| --- | --- | --- | --- |
| `0` | Police | `LEO` | LEO |
| `1` | Fire | `FIRE_EMS` | Fire/EMS |
| `2` | EMS | `FIRE_EMS` | Fire/EMS |
| `3` | Dispatch | `OTHER` | Not shown on an Active Units service display |
| Any other value | Unknown | `OTHER` | Not shown on an Active Units service display |

Server owners normally do not configure department names. The adapter copies department/agency, subdivision, rank, district, and group labels from the cache, but service classification uses `data.page`.

`Config.AdvancedServiceOverridesEnabled` and `Config.AdvancedServiceOverrides` provide a disabled-by-default fallback for legacy or customized caches with incorrect page data. Only enable this after confirming the cache classification is wrong.

## Per-display filters

| User-facing filter | Config/API value | Behavior |
| --- | --- | --- |
| LEO | `LEO` | Shows normalized law-enforcement units and matching calls. |
| Fire/EMS | `FIRE_EMS` | Shows normalized fire and EMS units and matching calls. |
| Both | `BOTH` | Shows both services and all calls, including unclassified calls. |

The filter is saved on each display. A police briefing room, apparatus bay, dispatch center, and emergency operations center can all use different filters without separate department lists.

## Unit fields

The adapter normalizes:

* ID and associated FiveM player ID, when available
* Callsign and display name
* Numeric and display status
* Service, agency, subdivision, rank, district, and group
* Location and postal
* World coordinates
* Assigned dispatch call ID
* Panic and dispatch flags
* Update timestamp and stale flag

Identifiers are included only when `Config.Debug = true`.

## Call caches

The pages remain separate because SonoranCADFiveM exposes two collections:

| Display page | Cache export | Normalized kind |
| --- | --- | --- |
| Emergency Calls | `GetEmergencyCache()` | `EMERGENCY_CALL` |
| Dispatch Calls | `GetCallCache()` | `DISPATCH_CALL` |

Dispatch calls with status `2` (`CLOSED`) are omitted. Emergency entries are displayed while they remain in the emergency cache.

A call's service is determined from its assigned normalized units:

* Only LEO assigned → `LEO`
* Only Fire/EMS assigned → `FIRE_EMS`
* Both services assigned → `BOTH`
* No classified assigned units → `OTHER`

Unassigned calls are not guessed from title, department, or description text. `BOTH` displays show them. Single-service displays hide them by default unless the corresponding `Config.CallClassificationFallback` option is enabled.

## Coordinates

* Unit markers use `unit.coordinates` or `unit.coords`.
* Call markers use X/Y/Z values in call `metaData`.
* The station marker uses the saved display position.

A record without usable X/Y coordinates remains available on its list page but does not appear on the map.

## Update flow

The adapter combines push-triggered refresh requests with centralized polling.

Unit events:

```text
SonoranCAD::pushevents:UnitLogin
SonoranCAD::pushevents:UnitLogout
SonoranCAD::pushevents:UnitUpdate
SonoranCAD::pushevents:UnitStatusUpdate
```

Call/assignment events:

```text
SonoranCAD::pushevents:DispatchEvent
SonoranCAD::pushevents:CallCacheUpdated
SonoranCAD::pushevents:EmergencyCacheUpdated
SonoranCAD::pushevents:UnitAttach
SonoranCAD::pushevents:UnitDetach
```

An event requests an early refresh. The server also polls units every 2 seconds and calls every 3 seconds by default to catch coordinate changes and older CAD behavior. One shared normalized cache and incremental client deltas are used regardless of display count.

Bodycam runtime updates are event-driven with a 1-second polling fallback. They are not
merged into either call cache. A single 4-second capture loop runs only for active
feeds with eligible subscribers.

## CAD restart and unavailable cache

When `sonorancad` stops, the adapter marks the connection unavailable and authorized clients show the full `CAD CONNECTION UNAVAILABLE` overlay. When it starts, the resource waits one second, redetects the exports, and requests refreshed caches.

If one cache call fails, the connection state becomes disconnected and the polling loop retries. Existing client records are not immediately erased, but the disconnected overlay covers the display.

Unchanged units receive an internal stale flag after `Config.StaleAfterSeconds`. Version 1.0.0 does not render a separate per-unit stale badge. Use the connection overlay, cache revision, and diagnostics when investigating data freshness.

## Radio menu relationship

`radio_fivem` was a source reference for the WarMenu, preview object, fine movement, rotation, confirmation, cancellation, synchronization, and cleanup patterns. Sonoran Station Displays ships the adapted code inside its own resource.

It does not:

* Call a radio menu export
* Require a Sonoran Radio resource name
* Require radio start order
* Modify a customer's radio resource
