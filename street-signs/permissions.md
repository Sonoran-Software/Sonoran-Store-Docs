---
title: Permissions
published: true
date: 2026-06-23T00:00:00.000Z
tags: null
editor: markdown
dateCreated: 2026-06-23T00:00:00.000Z
description: Configure Street Signs permissions for ACE, standalone, QBCore, or ESX.
---

# Permissions

Street Signs supports four permission modes:

* `ace`
* `standalone`
* `qb`
* `esx`

Set the mode in `config.lua`:

```lua
Config.PermissionMode = 'ace'
```

## ACE Permissions

When using ACE mode, Street Signs checks the ACE objects defined in `Config.AcePermissions`.

Default values:

```lua
Config.AcePermissions = {
    edit = 'sonoran.signs.edit',
    create = 'sonoran.signs.create',
    delete = 'sonoran.signs.delete',
    admin = 'sonoran.signs.admin'
}
```

Example `server.cfg` setup:

```cfg
add_ace group.admin sonoran.signs.admin allow
add_ace group.admin sonoran.signs.create allow
add_ace group.admin sonoran.signs.edit allow
add_ace group.admin sonoran.signs.delete allow
```

Recommended simplified setup:

```cfg
add_ace group.admin sonoran.signs.admin allow
```

The `admin` ACE acts as a full bypass for Street Signs admin functionality.

## Standalone Permissions

Standalone mode uses player identifiers stored in:

```lua
Config.StandaloneAllowed = {}
```

Example:

```lua
Config.StandaloneAllowed = {
    ['license:abc123'] = true,
    ['license:def456'] = true
}
```

Behavior in standalone mode:

* Allowed identifiers can create signs
* Allowed identifiers can delete signs
* Allowed identifiers can use admin tools
* A sign creator can edit their own sign
* Allowed identifiers can also edit any sign

## QBCore Permissions

QBCore mode uses the player's job and grade.

Example:

```lua
Config.PermissionMode = 'qb'

Config.QBJobs = {
    police = 3,
    dot = {0, 2},
    admin = 0
}
```

How grade rules work:

* A single number means that grade or higher
* A table means exact allowed grades only

Examples:

* `police = 3` means grade 3 or higher
* `dot = {0, 2}` means only grades 0 or 2

## ESX Permissions

ESX mode also uses job and grade values.

Example:

```lua
Config.PermissionMode = 'esx'

Config.ESXJobs = {
    police = 3,
    mechanic = {1, 2, 4}
}
```

This follows the same rule style as QBCore:

* A number means minimum grade
* A table means exact allowed grades

## What Each Permission Controls

### Create

Allows a user to place new signs using commands or supported creation flows.

### Edit

Allows a user to edit a sign's content.

In standalone mode, the original sign creator can edit their own sign even if they are not globally allowed to manage every sign.

### Delete

Allows a user to remove signs.

### Admin

Allows use of Street Signs admin tools and acts as a full bypass for the normal create, edit, and delete checks.

## In-World Editor Access

Players can open a nearby sign by walking up to it and pressing `E`, but Street Signs still validates edit access before the editor is allowed to open.

This means:

* Being near a sign is not enough by itself
* The server still checks whether the player is allowed to edit that sign

## Admin Tools

The full controller and some management tools are restricted to administrative access.

If a player needs access to:

* `/signcontroller`
* `/signlist`
* `/signrefresh`

they should have admin-level access in the mode you selected.
