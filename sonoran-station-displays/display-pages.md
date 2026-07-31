---
title: Display Pages and Rotation
published: true
date: 2026-07-28T00:00:00.000Z
tags: [page rotation, active units, live map, bodycams]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Configure display pages, ordering, synchronized runtime changes, and automatic rotation.
---

# Display Pages and Rotation

## Page IDs

| Page | ID | Source |
| --- | --- | --- |
| Active Units | `ACTIVE_UNITS` | Normalized on-duty units |
| Emergency Calls | `EMERGENCY_CALLS` | `GetEmergencyCache()` |
| Dispatch Calls | `DISPATCH_CALLS` | Non-closed entries from `GetCallCache()` |
| Live Map | `LIVE_MAP` | Unit, call, and station coordinates |
| Bodycams | `BODYCAMS` | Compatible SonoranCADFiveM bodycam runtime |

Emergency and dispatch calls remain separate because they come from different caches.

## Select, order, and default

Open a new or existing display's **Display Pages** menu. Enable one or more pages, choose a default, and use **Reorder Display Pages** to set rotation order.

The server removes invalid IDs and duplicates. At least one page is required. If the saved default is not enabled, validation selects a valid page.

## Automatic rotation

Each display saves `rotationEnabled`, `rotationInterval`, ordered `pageOrder`, and `defaultPage`. The default interval is 30 seconds; the accepted range is 5 to 300 seconds.

The footer shows a dot for every enabled page, highlights the current page, and animates progress. Rotation is unnecessary with one enabled page.

## Runtime synchronization

The server maintains runtime page state for a display. Clients rotate locally from that
state, while an eligible nearby manual page change is validated by the server and
broadcast to clients. After a manual change, the default hold is 10 seconds before
normal rotation resumes.

| Situation | Behavior |
| --- | --- |
| Rotation enabled with multiple pages | Advances at the saved interval. |
| Rotation disabled or one page | Remains on the current runtime page. |
| Empty page | Shows its empty state and continues normal rotation. |
| Nearby next/previous | Broadcast runtime page; hold rotation; do not persist. |
| Server `SetDisplayPage` export | Broadcast runtime page; do not persist. |
| DUI recreated after leaving range | Restores the current runtime snapshot. |
| Resource restart | Initializes runtime state from the saved default page. |
| Current page removed by settings | Selects the saved default or first valid page. |

Right and Left Arrow are the default next/previous mappings. The server checks display
proximity (with a small network tolerance), routing bucket, interior, page validity,
and cooldown.

## Independent list and bodycam pagination

Page rotation is separate from content pagination:

* Active-unit groups and call lists advance every 10 seconds when their data exceeds the saved limit.
* Bodycam feed pages advance at the display's bodycam interval, default 15 seconds.
* Page Down/Page Up changes bodycam feed pages while the display is on `BODYCAMS`.

Lowering a rotation interval does not make CAD data fresher. Cache refresh settings control data freshness.

## Pause and resume

The browser implements internal `PAUSE` and `RESUME` behavior, but the current customer menu, commands, events, and public exports do not expose temporary pause/resume. Disable saved page rotation when a display should remain fixed.
