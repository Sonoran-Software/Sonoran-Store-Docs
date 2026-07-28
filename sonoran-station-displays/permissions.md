---
title: Permissions
published: true
date: 2026-07-28T00:00:00.000Z
tags: [ace permissions, security, fivem]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Configure ACE or custom server-side permissions for viewing and managing Station Displays.
---

# Permissions

`Config.PermissionMode` supports `ace` (default) or `custom`. There is no built-in QBCore/ESX job mode.

## Logical actions

| Action | Default ACE | Controls |
| --- | --- | --- |
| `open` | `sonoran.display.view` | Open the management menu. |
| `view` | `sonoran.display.view` | Receive display/CAD/bodycam state and render displays. |
| `place` | `sonoran.display.place` | Create a validated nearby display. |
| `edit` | `sonoran.display.edit` | Change settings, name, or transform. |
| `delete` | `sonoran.display.delete` | Delete a validated nearby display. |
| `diagnostics` | `sonoran.display.diagnostics` | In-game/server diagnostics and full resync. |
| `forceRefresh` | `sonoran.display.edit` | Refresh a selected display. |
| `webhookTest` | `sonoran.display.webhook.test` | Send a rate-limited administrative webhook test. |

The shipped ACE configuration intentionally maps `open` and `view` to the same node and maps `forceRefresh` to the edit node. You may assign distinct ACE strings, but server authorization still resolves the logical operation to its backend capability. Empty nodes deny.

The server supplies a permission snapshot to build the menu, then revalidates every player action. A hidden or locally altered menu item grants no authority.

## Administrator

```ini
add_ace group.admin sonoran.display.view allow
add_ace group.admin sonoran.display.place allow
add_ace group.admin sonoran.display.edit allow
add_ace group.admin sonoran.display.delete allow
add_ace group.admin sonoran.display.diagnostics allow
add_ace group.admin sonoran.display.webhook.test allow
```

## View-only group

```ini
add_ace group.stationviewer sonoran.display.view allow
```

This group can receive data and see nearby displays. With the shipped shared `open` node it can also open the menu, but it receives no create/edit/delete options. Nearby page and map viewing controls require view permission, not management permission.

## Dispatcher management

```ini
add_ace group.dispatchmanagement sonoran.display.view allow
add_ace group.dispatchmanagement sonoran.display.edit allow
add_ace group.dispatchmanagement sonoran.display.diagnostics allow
```

This group can view, configure, move, refresh, and diagnose displays but cannot place or delete.

## ACE inheritance

```ini
add_principal group.admin group.dispatchmanagement
add_ace group.admin sonoran.display.place allow
add_ace group.admin sonoran.display.delete allow
```

FiveM evaluates allow/deny rules and principal inheritance. Inspect inherited deny rules when an expected grant fails.

## Custom mode

```lua
Config.PermissionMode = "custom"

Config.CustomPermission = function(source, action)
    return MyPermissionResource.CanUseStationDisplays(source, action)
end
```

The callback receives backend actions:

```text
view
place
edit
delete
diagnostics
webhookTest
```

Logical `open` resolves to `view`; `forceRefresh` resolves to `edit`. The callback runs inside `pcall`. Only boolean `true` allows; errors and other return values deny.

## Server validation

Depending on the request, the server checks:

* The current permission
* Display existence and maximum count
* Model allowlist
* Name, enum, page, and numeric ranges
* At least one enabled page
* Player proximity
* Routing bucket and interior context
* Page/action validity and cooldown
* Per-player event rate limits

Save/delete/move operations also validate the player against the saved/final position. A client cannot authorize itself by changing local state.

## Data access

Only view-authorized players receive saved display configurations, normalized on-duty units, both call caches, bodycam runtime metadata, and incremental updates. Grant view permission only to groups that may see the configured fields and bodycam content.

Caller normalization is disabled by default and caller text is not rendered. That does not make every call field suitable for a public-facing screen; review address and description settings.
