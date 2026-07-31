---
title: Permissions
published: true
date: 2026-07-31T00:00:00.000Z
tags: [ace permissions, security, fivem]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Configure ACE or custom server-side permissions for managing Station Displays.
---

# Permissions

`Config.PermissionMode` supports `ace` (default) or `custom`. There is no built-in QBCore/ESX job mode.

## Logical actions

| Action | Default ACE | Controls |
| --- | --- | --- |
| `menu` | `sonoran.stationdisplay.menu` | Open the administrative management menu. |
| `place` | `sonoran.stationdisplay.place` | Create a validated nearby display. |
| `edit` | `sonoran.stationdisplay.edit` | Change settings, name, or transform. |
| `delete` | `sonoran.stationdisplay.delete` | Delete a validated nearby display. |
| `diagnostics` | `sonoran.stationdisplay.diagnostics` | In-game/server diagnostics and full resync. |
| `forceRefresh` | `sonoran.stationdisplay.refresh` | Refresh a selected display. |
| `webhookTest` | `sonoran.stationdisplay.webhook.test` | Send a rate-limited administrative webhook test. |

Viewing displays does not require an ACE. Full sync, live cache updates, page scrolling,
map panning, and eligible bodycam subscriptions are public and remain subject to
server-side validity, distance, routing bucket, interior, cooldown, and rate-limit
checks.

The server supplies a permission snapshot to build the menu, then revalidates every
player action. A **No permission** entry or locally altered client grants no authority.

## Administrator

```ini
add_ace group.admin sonoran.stationdisplay.menu allow
add_ace group.admin sonoran.stationdisplay.place allow
add_ace group.admin sonoran.stationdisplay.edit allow
add_ace group.admin sonoran.stationdisplay.delete allow
add_ace group.admin sonoran.stationdisplay.diagnostics allow
add_ace group.admin sonoran.stationdisplay.refresh allow
add_ace group.admin sonoran.stationdisplay.webhook.test allow
```

## Menu-only group

```ini
add_ace group.stationdisplaymenu sonoran.stationdisplay.menu allow
```

This group can open the administrative menu and inspect nearby displays, but it cannot
place, configure, refresh, diagnose, or delete them. No group grant is needed merely to
view an in-world display.

## Dispatcher management

```ini
add_ace group.dispatchmanagement sonoran.stationdisplay.menu allow
add_ace group.dispatchmanagement sonoran.stationdisplay.edit allow
add_ace group.dispatchmanagement sonoran.stationdisplay.diagnostics allow
add_ace group.dispatchmanagement sonoran.stationdisplay.refresh allow
```

This group can open the menu, configure, move, refresh, and diagnose displays but cannot
place or delete them.

## ACE inheritance

```ini
add_principal group.admin group.dispatchmanagement
add_ace group.admin sonoran.stationdisplay.place allow
add_ace group.admin sonoran.stationdisplay.delete allow
add_ace group.admin sonoran.stationdisplay.webhook.test allow
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
menu
place
edit
delete
diagnostics
refresh
webhookTest
```

The `forceRefresh` logical action resolves to the `refresh` backend action. The callback
runs inside `pcall`. Only boolean `true` allows; errors and other return values deny.

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

Every player receives sanitized display definitions and live board data. There is no
viewing ACE. Bodycam image delivery additionally requires an eligible subscription and
passes distance, routing bucket, interior, feed, payload, and rate-limit validation.

Caller normalization is disabled by default and caller text is not rendered. That does not make every call field suitable for a public-facing screen; review address and description settings.
