---
title: Display Pages and Rotation
published: true
date: 2026-07-27T00:00:00.000Z
tags: [page rotation, active units, live map]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Configure Sonoran Station Displays pages, ordering, defaults, automatic rotation, and manual controls.
---

# Display Pages and Rotation

## Page IDs

Use these exact IDs in `config.lua` and developer APIs:

| Page | ID | Data |
| --- | --- | --- |
| Active Units | `ACTIVE_UNITS` | On-duty normalized units matching the display service filter |
| Emergency Calls | `EMERGENCY_CALLS` | Incoming entries from `GetEmergencyCache()` |
| Dispatch Calls | `DISPATCH_CALLS` | Non-closed entries from `GetCallCache()` |
| Live Map | `LIVE_MAP` | Unit, call, and station coordinate markers |

Emergency and Dispatch Calls are genuine separate pages backed by different SonoranCADFiveM caches.

## Select and order pages

1. Open a new or existing display's **Display Configuration**.
2. Select **Display Pages**.
3. Enable one or more pages.
4. Choose **Default Page**.
5. Open **Reorder Display Pages**.
6. Select a page and move it up or down.
7. Save the display.

At least one page must remain enabled. Invalid IDs, duplicates, and an invalid default page are corrected by server validation.

## Automatic rotation

Each display saves:

* `rotationEnabled`
* `rotationInterval`
* Ordered `pageOrder`
* `defaultPage`

The server default is:

```lua
Config.DefaultRotationInterval = 30
```

Allowed saved range:

```text
5–300 seconds
```

Example:

```text
Page order: ACTIVE_UNITS → EMERGENCY_CALLS → LIVE_MAP
Default page: ACTIVE_UNITS
Rotation: enabled
Interval: 30 seconds
```

The footer shows one dot for each enabled page, highlights the current page, and animates a rotation progress bar.

## Rotation behavior

| Situation | Behavior |
| --- | --- |
| More than one page and rotation enabled | Advances at the saved interval. |
| One page enabled | Remains on that page; no automatic advance is needed. |
| Rotation disabled | Remains on the current/default page. |
| Current page has no matching data | Shows that page's empty state and continues normal rotation. |
| DUI is recreated after leaving render distance | Starts from the saved default page. |
| Resource/client restart | Runtime page resets to the saved default page. |
| Settings change removes the current page | Moves to the first valid page. |

Rotation timing is client-local. Multiple clients can be at different points in the same display's cycle after joining or manually changing pages.

## Manual next and previous

The nearest display within the global interaction distance can be controlled with:

| Default key | Command |
| --- | --- |
| Right Arrow | `stationdisplay_next` |
| Left Arrow | `stationdisplay_previous` |

These commands change only the local DUI state and reset its rotation timer. They do not persist the current page.

The server `SetDisplayPage` export can broadcast a runtime page to all authorized clients, but that runtime page also is not persisted.

## Pause and resume

The internal renderer implements `PAUSE` and `RESUME` actions. Version 1.0.0 does not expose them through the shipped WarMenu, a command, an event, or a public export. Customers can enable or disable automatic rotation in saved display settings, but there is no supported temporary pause control.

## List pagination

Large unit and call collections paginate independently every 10 seconds:

* The Active Units row budget is divided across visible groups.
* Call pages use the saved calls-per-page limit.
* Pagination is automatic and has no manual page control.
* A page's list pagination is separate from display-page rotation.

Lowering the display rotation interval does not make CAD data fresher. Data freshness comes from cache events and the unit/call refresh intervals.
