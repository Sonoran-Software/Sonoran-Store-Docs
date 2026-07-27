---
title: Performance
published: true
date: 2026-07-27T00:00:00.000Z
tags: [performance, dui, scaling]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Scale Sonoran Station Displays by tuning DUI lifecycle, distances, cache polling, row limits, and map usage.
---

# Performance

## Shared data design

Sonoran Station Displays does not poll SonoranCADFiveM once per television.

```text
SonoranCADFiveM exports/events
→ one normalized server cache
→ full sync or shared incremental deltas
→ one client cache
→ per-display filtering
→ nearby DUI renderers
```

Unit and call polls are centralized. Display count does not multiply server export calls.

## DUI lifecycle

Every client checks display distance twice per second.

When a display enters range:

1. The object model loads.
2. A frozen display object is created.
3. One 1280×720 DUI and runtime texture are created.
4. The current filtered state is sent.
5. The surface is drawn while active.

When it leaves range, the client waits `Config.DuiDestroyDelay` (3 seconds by default), then deletes the object and DUI. The delay prevents repeated create/destroy cycles near the boundary.

Object and DUI cleanup also runs when the resource stops.

## Pool limit

`Config.MaximumActiveDuis = 8` is a hard per-client limit. If the limit is reached, additional in-range displays cannot create a DUI until capacity becomes available.

Keep no more than eight displays inside one client's render range with the default configuration. For low-end clients or large 3D interiors, use fewer. This is a resource limit, not a tested player-count claim.

## Render distance

Default:

```lua
Config.RenderDistance = 75.0
```

Per-display range:

```text
5–250 meters
```

Use the shortest distance that allows the intended viewing area. A longer distance:

* Creates objects and DUIs earlier
* Keeps them alive longer
* Makes dense placements more likely to reach the pool limit
* Increases the number of surfaces drawn each frame

## Incremental updates

The server compares normalized cache records by ID.

* Added, changed, and removed records become incremental client deltas.
* Identical record content does not receive another delta.
* The DUI receives a full display-state payload only when state changes, on explicit refresh, or on a page change.
* Rotation progress state is checked at most once per second; the browser animates smoothly between messages.

Coordinates use the same unit delta stream. There is no per-screen coordinate loop.

## Recommended intervals

| Setting | Default | Guidance |
| --- | --- | --- |
| `UnitRefreshInterval` | 2000 ms | Keep at 2 seconds unless a measured integration issue requires change. |
| `CallRefreshInterval` | 3000 ms | Keep at 3 seconds for normal call volumes. |
| `DefaultRotationInterval` | 30 seconds | Page presentation only; does not affect cache freshness. |
| `DuiDestroyDelay` | 3000 ms | Raise only if clients repeatedly cross a render boundary. |

Push events already request early refreshes. Lower polling intervals increase export calls and delta processing without making every upstream CAD change arrive sooner.

## Interface resolution and row limits

The shipped interface is 1280×720. Raising `DuiWidth` or `DuiHeight` increases per-DUI browser/GPU memory and should be tested on representative client hardware.

Higher unit/call limits increase the number of DOM rows and cards. Automatic 10-second list pagination provides a better layout than very large page limits.

## Live Map cost

The map redraws its canvas when state changes and projects every visible marker. To reduce cost or clutter:

* Disable call markers on displays that do not need them.
* Reduce the number of displays simultaneously showing the map.
* Keep the default DUI resolution.
* Reduce render distance in dense stations.

`MapRefreshInterval` is not active in version 1.0.0, so changing it does not affect cost.

## Routing buckets and interiors

A display with a saved routing bucket is created only when the client's local bucket matches. A display with an interior ID is created only when the player's current interior matches.

These checks prevent irrelevant objects and DUIs in other world contexts. New displays automatically capture a nonzero interior ID.

## Diagnose high client usage

1. Open `/stationdisplay`.
2. Select **Mounted Displays → View Diagnostics**.
3. Check **Active DUIs** and **Nearby Displays**.
4. Compare active DUI count with `MaximumActiveDuis`.
5. Move outside render distance and verify the count drops after the destroy delay.
6. Check whether several displays overlap at high render distances.
7. Temporarily reduce render distances or call markers and retest.
8. Check the client F8 console for repeated DUI/model errors.

Do not use page rotation speed as a data-refresh control.

## Release testing

Before deployment, test:

* One and several identical model instances with different content
* Pool limit and out-of-range cleanup
* Resource restart and player join
* CAD present, stopped, restarted, and incompatible
* Routing buckets and interiors
* Long labels and maximum row counts
* Auto-fit, fixed-region, and follow-units maps
* The server's representative client hardware and map assets

The source repository's static checks cannot verify native DUI orientation, model offsets, OneSync contexts, or client performance inside FiveM.
