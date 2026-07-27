---
title: Display Management
published: true
date: 2026-07-27T00:00:00.000Z
tags: [display placement, menu, controls]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Place, configure, move, preview, refresh, and delete mounted Sonoran Station Displays.
---

# Display Management

## Open the menu

Run:

```text
/stationdisplay
```

The command name comes from `Config.AdminCommand`. If a server owner changes that setting, use the configured command.

Menu path:

```text
Sonoran Station Displays → Mounted Displays
```

The menu is a self-contained WarMenu adapted from the `radio_fivem` speaker-menu implementation. It does not use radio menu exports.

## Place a display

1. Open **Mounted Displays**.
2. Select **Place New Display**.
3. Enter a display name.
4. Select **Flat Screen TV** or **Laptop Display**.
5. Select `LEO`, `Fire/EMS`, or `Both`.
6. Open **Display Pages** and select at least one page.
7. Choose the default page and reorder the enabled pages if needed.
8. Configure rotation, grouping, sorting, row limits, map behavior, and render distance.
9. Select **Continue to Placement**.
10. Position and rotate the translucent preview.
11. Select **Confirm Placement**.

The server checks `place`, validates the complete display, and requires the final position to remain within `Config.MaximumPlacementDistance` of the player.

## Placement controls

| Control | Action |
| --- | --- |
| Numpad 4 / Numpad 6 | Move on the X axis |
| Numpad 8 / Numpad 2 | Move on the Y axis |
| Page Up / Page Down | Move on the Z axis |
| **Rotation Axis** menu option | Select X, Y, or Z |
| Numpad 7 / Numpad 9 | Rotate around the selected axis |
| Shift | Increase movement/rotation step by `0.001`, up to `2.0` |
| Ctrl | Decrease movement/rotation step by `0.001`, down to `0.001` |
| **Reset Rotation** | Set X/Y to zero and Z to the player's current heading |
| Enter on **Confirm Placement** | Save the position and rotation |
| **Cancel Placement** or Backspace | Delete the preview and restore an existing display |

The initial step is `0.05`. Shift and Ctrl change the step when released; they are not held fine-adjustment modifiers.

## Placement behavior and limits

* A new preview begins at the first camera ray hit within 5 meters. If there is no hit, it starts 2 meters in front of the player.
* The gameplay camera remains under normal player control.
* The preview is translucent, frozen, and has collision disabled.
* The saved object is frozen, invincible, and has collision enabled.
* There is no automatic wall snap, ground snap, or surface alignment.
* The model is selected before placement; placement mode does not cycle models.
* A display must remain within 15 meters by default.
* New displays automatically save the current interior ID when the player is inside an interior.
* New displays do not automatically save a routing bucket.

## View nearby displays

Open:

```text
Mounted Displays → View Nearby Displays
```

The selector lists displays within `Config.MenuNearbyDistance` (15 meters by default), ordered by distance. Displays restricted to another routing bucket or interior are excluded.

Select a display, then open **Display Options**.

## Edit display settings

**Edit Display** allows:

* Rename
* Service selection
* Enable/disable pages
* Page order
* Default page
* Automatic page rotation
* Rotation interval
* Unit grouping and sorting
* Hide unavailable units
* Units and calls per page
* Map mode, marker toggles, center, and zoom
* Render distance

The model cannot be changed after creation in the shipped menu. Create a replacement display if another model is needed.

Select **Save Display Settings** to persist changes. The server requires `edit` and checks that the player is near the display's saved position.

## Move or rotate a display

1. Select **Move Display**.
2. The existing object is hidden and a translucent preview appears at its saved transform.
3. Use the placement controls.
4. Confirm to save, or cancel to restore the original object.

## Service filters

### LEO

Use for a police briefing room or law-enforcement operations area. It shows on-duty units whose CAD page classifies as police.

### Fire/EMS

Use for an apparatus bay, fire station, or EMS room. It combines fire and EMS cache pages into `FIRE_EMS`.

### Both

Use for a combined dispatch center or emergency operations center. The Active Units page separates LEO and Fire/EMS when grouping by service.

The filter is saved independently on every display. Agency and subdivision labels still appear when the CAD cache provides them.

## Preview and force refresh

**Preview Display**:

* Keeps the object/DUI active for 10 seconds even if it is outside normal render distance
* Sets the local display to its default page
* Marks the display location with a temporary blue marker

**Force-Refresh Display**:

* Requires `edit`
* Sends a full shared cache sync to the requesting player
* Asks every authorized client currently rendering that display to resend its DUI state

Force refresh does not force SonoranCADFiveM itself to rebuild its caches.

## Manual page controls

While within `Config.InteractionDistance` (3 meters by default):

| Default key | Command | Action |
| --- | --- | --- |
| Right Arrow | `stationdisplay_next` | Next page on the nearest display |
| Left Arrow | `stationdisplay_previous` | Previous page on the nearest display |

Players can rebind these commands in FiveM key settings. The page change is local to that client's active DUI and is not persisted.

## Delete a display

1. Open the nearby display's **Display Options**.
2. Select **Delete Display**.
3. Confirm **Yes, Delete Display**.

Deletion requires `delete`, a nearby player position, and server validation. The display is removed from `data/displays.json` and from authorized clients.

