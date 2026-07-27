---
title: Permissions
published: true
date: 2026-07-27T00:00:00.000Z
tags: [ace permissions, security, fivem]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Configure ACE or custom server-side permissions for viewing and managing Sonoran Station Displays.
---

# Permissions

Sonoran Station Displays supports:

* `ace` — default FiveM ACE checks
* `custom` — a server-side function in `config.lua`

The resource does not implement QBCore or ESX job modes.

## Permission actions

| Action | Default ACE | Controls |
| --- | --- | --- |
| `view` | `sonoran.display.view` | Receives saved display configurations and shared CAD cache data; opens the menu and views displays. |
| `place` | `sonoran.display.place` | Creates a display after server validation and proximity checks. |
| `edit` | `sonoran.display.edit` | Changes settings, moves/renames displays, requests a selected-display refresh, and changes broadcast runtime pages through supported server APIs. |
| `delete` | `sonoran.display.delete` | Deletes a nearby display. |
| `diagnostics` | `sonoran.display.diagnostics` | Opens display diagnostics or runs the server diagnostic command as a player. |

There is no separate ACE for opening the management menu. The `view` permission controls both menu access and receipt of customer display/CAD state.

There is no separate force-refresh ACE. Force refresh uses `edit`.

## Administrator example

```cfg
add_ace group.admin sonoran.display.view allow
add_ace group.admin sonoran.display.place allow
add_ace group.admin sonoran.display.edit allow
add_ace group.admin sonoran.display.delete allow
add_ace group.admin sonoran.display.diagnostics allow
```

## Dispatcher-management group

This group can view, edit, and diagnose displays, but cannot place or delete them:

```cfg
add_ace group.dispatchmanagement sonoran.display.view allow
add_ace group.dispatchmanagement sonoran.display.edit allow
add_ace group.dispatchmanagement sonoran.display.diagnostics allow
```

## Department supervisors

This group can view and edit nearby displays, but cannot delete them or open diagnostics:

```cfg
add_ace group.departmentsupervisor sonoran.display.view allow
add_ace group.departmentsupervisor sonoran.display.edit allow
```

## ACE inheritance

Use `add_principal` when one group should inherit another group's grants:

```cfg
add_principal group.admin group.dispatchmanagement
add_ace group.admin sonoran.display.place allow
add_ace group.admin sonoran.display.delete allow
```

ACE rules are evaluated by FiveM. Review deny rules and principal inheritance if a player unexpectedly lacks access.

## Custom permission mode

Set:

```lua
Config.PermissionMode = "custom"
```

Then implement the server-side callback:

```lua
Config.CustomPermission = function(source, action)
    -- Return true only when this player may perform this action.
    return MyPermissionResource.CanUseStationDisplays(source, action)
end
```

`action` is one of:

```text
view
place
edit
delete
diagnostics
```

The callback runs inside `pcall`. An error or any result other than boolean `true` denies access.

## Server-side validation

Hiding a menu item is not the security boundary. Player-originated save, delete, refresh, menu, sync, and diagnostic requests are checked by the server.

Save requests also validate:

* Permission for create or edit
* Maximum display count
* Model allowlist
* Name length
* Position and rotation
* Enum and numeric ranges
* At least one enabled page
* Player proximity
* Routing bucket, when saved
* Event rate limit

Delete requests require delete permission and proximity. Refresh requires edit permission. A client cannot gain authority by opening the menu or changing local state.

## Viewing and data access

Only players with `sonoran.display.view` receive:

* Saved display configurations
* Normalized on-duty unit cache
* Emergency-call cache
* Dispatch-call cache
* Shared incremental cache updates

Grant this permission only to groups that should see the fields enabled in `Config.VisibleCallFields`. Caller information is disabled by default.

