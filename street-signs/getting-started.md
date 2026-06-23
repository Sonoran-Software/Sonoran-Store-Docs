---
title: Getting Started
published: true
date: 2026-06-23T00:00:00.000Z
tags: null
editor: markdown
dateCreated: 2026-06-23T00:00:00.000Z
description: Install and start using Street Signs on your FiveM server.
---

# Getting Started

## Acquire the Script

After purchasing Street Signs through the Sonoran Store, download the package from the Keymaster account that owns the asset. Extract the package before moving the files to your server.

If you need help locating your purchase files, see [Accessing Tebex Assets](../general/tebex-assets.md).

## Install the Script

1. Copy both included folders into your server's resources directory:

```text
sonoran-streetsigns
sonoran-streetsigns_helper
```

2. In the `sonoran-streetsigns` folder, rename:

```text
config.CHANGEME.lua -> config.lua
```

3. Add the resource to your `server.cfg`:

```cfg
ensure sonoran-streetsigns
```

4. Restart the resource or your server.

## Auto Config Behavior

If `config.lua` is missing, Street Signs will try to create it automatically from `config.CHANGEME.lua`.

Even with that fallback available, the recommended setup is still to rename the file yourself and review the configuration before going live.

## Basic First-Time Setup

Before using Street Signs in-game, review these areas in `config.lua`:

* `Config.PermissionMode`
* `Config.EnabledPacks`
* `Config.DefaultTheme`
* `Config.CAD`
* `Config.Power`
* `Config.Webhooks`
* `Config.Updater`

The full setting reference is available on [Configuration Reference](configuration-reference.md).

## First In-Game Use

Once the resource is running and permissions are configured:

1. Create a sign with `/signcreate [id] [label optional]`
2. Walk up to the placed sign
3. Press `E` to open the editor if you have edit access
4. Make your changes and save them

Administrators can also open the full controller with:

```text
/signcontroller
```

## What Street Signs Includes

Street Signs ships with a built-in base pack and optional expansion-ready packs.

Built-in controller options currently include:

* Highway Sign Only
* US Highway Sign
* UK Style
* Billboard Flat
* Billboard V Triangle
* Speed And Warning Signs
* Trailer Construction Board
* Arrow Board

Some of these are controlled by `Config.EnabledPacks`. If a pack is disabled, its sign types will not be available until you enable that pack.

## Notes About Sign Display

Street Signs can render signs using its built-in display pipeline and can also fall back to floating preview text when needed.

For most customers, the main takeaway is simple:

* Signs are persistent
* Signs sync to connected players
* Nearby signs can be edited in-game if the player has permission

## Recommended Next Steps

After installation, continue with:

{% content-ref url="configuration-reference.md" %}
[configuration-reference.md](configuration-reference.md)
{% endcontent-ref %}

{% content-ref url="permissions.md" %}
[permissions.md](permissions.md)
{% endcontent-ref %}

{% content-ref url="commands-and-usage.md" %}
[commands-and-usage.md](commands-and-usage.md)
{% endcontent-ref %}
