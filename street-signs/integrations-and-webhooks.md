---
title: Integrations and Webhooks
published: true
date: 2026-06-23T00:00:00.000Z
tags: null
editor: markdown
dateCreated: 2026-06-23T00:00:00.000Z
description: Configure Street Signs integrations including CAD, power support, and Discord webhooks.
---

# Integrations and Webhooks

## Sonoran CAD

Street Signs includes configuration for Sonoran CAD related features.

Relevant settings:

```lua
Config.CAD = {
    enabled = true,
    syncOnStartup = true,
    shareIconCatalog = false,
    allowRemoteGridUpdates = true,
    allowRemoteImageUpdates = true
}
```

Use these settings if your Street Signs workflow includes CAD-connected updates or related remote control features.

If you do not use CAD-related features, you can disable them:

```lua
Config.CAD.enabled = false
```

## Power Support

Street Signs also includes a power integration section:

```lua
Config.Power = {
    enabled = false,
    mode = 'sonoran_powergrid'
}
```

If your signs do not need power-related behavior, leave this disabled.

## Discord Webhooks

Street Signs can post webhook messages for:

* Sign create, update, and delete actions
* Blocked banned-word attempts

Enable the system with:

```lua
Config.Webhooks.enabled = true
```

### Sign Change Webhooks

Configure the normal sign activity webhook here:

```lua
Config.Webhooks.signChanges = {
    url = '',
    username = 'Street Signs',
    avatarUrl = '',
    color = 3447003
}
```

This webhook can be used to log:

* New signs
* Text changes
* Theme changes
* Enables and disables
* Deletes

### Banned Word Alert Webhooks

Configure moderation alerts here:

```lua
Config.Webhooks.bannedWordAlerts = {
    url = '',
    username = 'Street Signs Moderation',
    avatarUrl = '',
    color = 15158332,
    mentionRoleIds = {
        -- '123456789012345678'
    }
}
```

This is useful if you want staff to be alerted when a player attempts to save blocked content.

### Legacy Webhook Fields

Street Signs still includes these older fallback fields:

* `Config.Webhooks.signUpdates`
* `Config.Webhooks.audit`

For new setups, prefer `Config.Webhooks.signChanges`.

## Auto Update

Street Signs includes updater support through:

* `sonoran-streetsigns`
* `sonoran-streetsigns_helper`

Auto update is controlled by:

```lua
Config.Updater = {
    EnableAutoUpdate = false
}
```

If enabled, make sure the helper resource is present and command permissions are allowed for the resource.

Example:

```cfg
add_ace resource.sonoran-streetsigns command allow
add_ace resource.sonoran-streetsigns_helper command allow
```

If you do not want automatic updates, leave the feature disabled.
