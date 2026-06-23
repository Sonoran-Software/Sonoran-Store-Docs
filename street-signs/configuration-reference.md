---
title: Configuration Reference
published: true
date: 2026-06-23T00:00:00.000Z
tags: null
editor: markdown
dateCreated: 2026-06-23T00:00:00.000Z
description: Explanation of the Street Signs configuration options.
---

# Configuration Reference

Street Signs uses `config.lua` for customer-facing configuration.

## Core Settings

### `Config.PermissionMode`

Controls how Street Signs checks access for creating, editing, deleting, and admin tools.

Available options:

* `ace`
* `standalone`
* `qb`
* `esx`

### `Config.DefaultTheme`

Sets the default color theme used when a new sign is created.

Example:

```lua
Config.DefaultTheme = 'amber'
```

### `Config.EnabledPacks`

Turns built-in sign packs on or off.

Default values:

```lua
Config.EnabledPacks = {
    base = true,
    billboard = false,
    street = false,
    vehicles = false
}
```

Pack summary:

* `base`: Core built-in signs
* `billboard`: Billboard-style displays
* `street`: Street and warning sign options
* `vehicles`: Vehicle-mounted board options

### `Config.EnableFloatingTextPreview`

If enabled, Street Signs can show floating preview text when the normal display mode is not being used.

## Interaction and Display Settings

### `Config.InteractionDistance`

How close a player must be to interact with a nearby sign.

Default:

```lua
Config.InteractionDistance = 3.0
```

### `Config.MaxLinesPerSign`

Maximum number of text lines the script will allow on a sign.

Default:

```lua
Config.MaxLinesPerSign = 8
```

### `Config.DefaultLineMaxLength`

Default character limit used for individual sign lines.

Default:

```lua
Config.DefaultLineMaxLength = 64
```

## CAD Settings

### `Config.CAD.enabled`

Enables or disables Sonoran CAD related support.

### `Config.CAD.syncOnStartup`

Controls whether CAD-related sign synchronization actions should happen during startup.

### `Config.CAD.shareIconCatalog`

Used for icon-related workflows when applicable.

### `Config.CAD.allowRemoteGridUpdates`

Allows remote grid-style updates when the related workflow is in use.

### `Config.CAD.allowRemoteImageUpdates`

Allows remote image-style updates when the related workflow is in use.

Default block:

```lua
Config.CAD = {
    enabled = true,
    syncOnStartup = true,
    shareIconCatalog = false,
    allowRemoteGridUpdates = true,
    allowRemoteImageUpdates = true
}
```

## Power Integration Settings

### `Config.Power.enabled`

Turns power integration support on or off.

### `Config.Power.mode`

Selects the expected power integration mode.

Default block:

```lua
Config.Power = {
    enabled = false,
    mode = 'sonoran_powergrid'
}
```

## Webhook Settings

### `Config.Webhooks.enabled`

Master toggle for Discord webhook support.

### `Config.Webhooks.username`

Default webhook username for Street Signs posts.

### `Config.Webhooks.avatarUrl`

Default avatar image URL for webhook messages.

### `Config.Webhooks.signChanges`

Controls webhook messages for sign create, update, and delete events.

Important fields:

* `url`
* `username`
* `avatarUrl`
* `color`

### `Config.Webhooks.bannedWordAlerts`

Controls webhook messages for blocked banned-word attempts.

Important fields:

* `url`
* `username`
* `avatarUrl`
* `color`
* `mentionRoleIds`

Default block:

```lua
Config.Webhooks = {
    enabled = false,
    username = 'Street Signs',
    avatarUrl = '',
    signUpdates = '',
    audit = '',
    signChanges = {
        url = '',
        username = 'Street Signs',
        avatarUrl = '',
        color = 3447003
    },
    bannedWordAlerts = {
        url = '',
        username = 'Street Signs Moderation',
        avatarUrl = '',
        color = 15158332,
        mentionRoleIds = {
            -- '123456789012345678'
        }
    }
}
```

## Auto Update Settings

### `Config.Updater.EnableAutoUpdate`

Enables the updater flow.

Default:

```lua
Config.Updater = {
    EnableAutoUpdate = false
}
```

If you turn auto-update on, make sure the helper resource is installed and the required command ACE permissions are granted. See [FAQ and Troubleshooting](faq-and-troubleshooting.md).

## Permissions Configuration

### `Config.AcePermissions`

Used when `Config.PermissionMode = 'ace'`.

```lua
Config.AcePermissions = {
    edit = 'sonoran.signs.edit',
    create = 'sonoran.signs.create',
    delete = 'sonoran.signs.delete',
    admin = 'sonoran.signs.admin'
}
```

### `Config.QBJobs`

Used when `Config.PermissionMode = 'qb'`.

The value can be:

* A minimum grade number
* A list of exact allowed grades

Example:

```lua
Config.QBJobs = {
    police = 3,
    dot = {0, 2},
    admin = 0
}
```

This means:

* `police = 3`: police grade 3 or higher
* `dot = {0, 2}`: only DOT grades 0 or 2
* `admin = 0`: admin grade 0 or higher

### `Config.ESXJobs`

Used when `Config.PermissionMode = 'esx'`.

Example:

```lua
Config.ESXJobs = {
    police = 3,
    mechanic = {1, 2, 4}
}
```

### `Config.StandaloneAllowed`

Used when `Config.PermissionMode = 'standalone'`.

Add allowed player identifiers here:

```lua
Config.StandaloneAllowed = {
    ['license:abc123'] = true
}
```

## Content Moderation

### `Config.BannedWords`

This list blocks sign content that matches banned phrases or words.

Use this section to adjust moderation rules for your community.

Important note:

Street Signs uses substring matching, so broad or short entries may block more than you expect. Review changes carefully before using the script on a live server.

## Built-In Defaults You Normally Do Not Need To Change

Street Signs also applies built-in defaults for its controller URL, display pipeline, and some internal behavior. Most customers should leave those alone and focus on the documented `config.lua` options above.
