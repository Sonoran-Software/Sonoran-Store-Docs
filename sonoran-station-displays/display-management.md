---
title: Display Management
published: true
date: 2026-07-28T00:00:00.000Z
tags: [display placement, menu, controls]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Place, configure, move, preview, refresh, and delete mounted Station Displays.
---

# Display Management

## Open the menu

Run `/stationdisplay`, or the command configured by `Config.AdminCommand`.

The self-contained WarMenu is adapted from Sonoran's `radio_fivem` placement patterns. It does not call radio exports or require the radio resource. While the server supplies the latest permission and display snapshots, the menu shows a non-actionable **Loading Station Displays...** state. The compact blue header, highlighted selection, descriptions, and control hints then remain consistent throughout every submenu.

## Place a display

1. Open **Mounted Displays > Place Display**.
2. Enter a name and choose an allowed model.
3. Choose `LEO`, `Fire/EMS`, or `Both`.
4. Enable at least one page, select the default, and arrange the page order.
5. Configure rotation, grouping, sorting, list limits, map/bodycam behavior, theme, render distance, and interaction distance.
6. Choose **Continue to Placement**.
7. Position and rotate the translucent preview.
8. Confirm placement.

The server rechecks place permission, model, settings, player/world context, and the final position. The default maximum placement distance is 15 meters.

## Placement controls

The placement overlay shows the resolved FiveM control labels for the current client. Default keyboard mappings correspond to:

| Control group | Action |
| --- | --- |
| Numpad horizontal controls | Move X |
| Numpad vertical controls | Move Y |
| Page Up / Page Down | Move Z |
| **Rotation Axis** | Select X, Y, or Z |
| Numpad rotation controls | Rotate the selected axis |
| Shift release | Increase step by `0.001`, up to `2.0` |
| Ctrl release | Decrease step by `0.001`, down to `0.001` |
| **Reset Rotation** | Zero X/Y and use the player's current heading for Z |
| **Confirm Placement** | Save |
| **Cancel Placement** or Back | Remove the preview and restore an existing object |

The initial movement/rotation step is `0.05`. Shift and Ctrl change the step when released; they are not hold-to-modify controls.

Placement starts at the first camera ray hit within 5 meters, or 2 meters in front of the player when no hit is found. The preview is translucent, frozen, and non-colliding. A saved object is frozen, invincible, and colliding. There is no automatic wall/ground snap.

New displays save the current interior when applicable. The menu does not assign a routing bucket. Existing or externally provisioned bucket values are still enforced.

## View and edit nearby displays

Open **Mounted Displays > Nearby Displays**. Results inside `Config.MenuNearbyDistance` (15 meters by default) are ordered by distance and filtered for the current interior/routing context.

**Configure Display** can change:

* Name and service
* Theme
* Enabled pages, default page, and order
* Main rotation and interval
* Unit grouping/sorting and unavailable-unit visibility
* Unit/call list limits
* Live Map mode, markers, center, and zoom
* Bodycam layout, feed limit, and bodycam page interval
* Render and interaction distances

The shipped menu cannot change a display model after creation. Replace the display when a different model is needed.

## Move a display

Choose **Reposition Display**. The existing object is hidden while its translucent preview is moved. Confirming persists the new transform; cancelling restores the original object. The server rechecks edit permission, proximity, and world context.

## Preview and refresh

**Preview Display** keeps the local renderer active for 10 seconds, switches it to its default page, and places a temporary blue marker at the display.

**Force Refresh**:

* Requires the configured refresh permission (`sonoran.stationdisplay.refresh` by default)
* Requests a full shared cache snapshot for the player
* Tells clients rendering that display to resend its DUI state

It does not force SonoranCADFiveM to rebuild its caches.

## Nearby viewer controls

Target selection considers distance, view angle, and the display surface, then checks line of sight and world context. When several displays are in range, the best combined distance/angle score wins; this is not always the physically nearest object.

| Page/context | Default keys | Behavior |
| --- | --- | --- |
| Any multi-page display | Right / Left Arrow | Next / previous display page |
| Live Map | Numpad 8/2/4/6, Numpad 5 | Temporary synchronized pan and reset |
| Bodycams | Page Down / Page Up | Next / previous bodycam feed page |

The default interaction distance is 2.5 meters and can be saved per display. Page
changes are validated by the server, broadcast to clients, and hold automatic rotation
for 10 seconds by default. They are runtime-only and do not change the saved default
page.

Map panning is also runtime-only and synchronized. Bodycam subpage navigation is local to the targeted DUI. FiveM key settings can rebind every registered command.

The prompt is hidden when no action applies. A single-page display can still show controls when its Live Map or Bodycams page has context-specific actions.

## Delete

Choose **Display Options > Delete Display** and confirm. Deletion requires delete permission, proximity, and server validation. The record is removed from persistence and authorized clients.
