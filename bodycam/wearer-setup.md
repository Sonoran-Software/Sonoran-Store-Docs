---
title: Wearer Setup
published: true
date: 2026-08-18T00:00:00.000Z
tags: [fivem, bodycam, wearer eligibility, eup, inventory]
editor: markdown
dateCreated: 2026-08-18T00:00:00.000Z
description: Choose who can wear a Bodycam using permissions, framework jobs, inventory, and server-observed EUP rules.
---

# Bodycam wearer setup

A player must be an eligible, networked wearer before a recording can start. Eligibility and recording permission are separate: the wearer must pass the policy in eventsBodycam/config/wearers.lua and must also have bodycam.record through ACE or a configured framework job grade.

## Eligibility modes

`Config.Wearers.EligibilityMode` supports three values:

| Mode | Behavior |
| --- | --- |
| permission | Requires bodycam.use through ACE or a configured on-duty job. Inventory and EUP do not replace this check. |
| any | Accepts the wearer when the permission check or any enabled inventory/EUP check passes. |
| all | Requires the permission check and every enabled inventory/EUP check to pass. |

The shipped mode is `permission`. `RequireOnDuty` is true, so a framework player explicitly reported as off duty is denied even if another enabled check would otherwise pass.

## Shipped configuration

~~~lua
Config.Wearers = {
    EligibilityMode = 'permission',
    RequireOnDuty = true,

    Inventory = {
        Enabled = false,
        Item = 'bodycam'
    },

    EUP = {
        Enabled = false,
        Rules = {}
    }
}
~~~

## Permission and job eligibility

The permission check accepts bodycam.use through ACE. When Events Core is configured for ESX, QBCore, or Qbox, an on-duty job listed in eventsBodycam/config/permissions.lua can also satisfy the check.

The shipped job list contains police, sheriff, ambulance, and fire. Review both the wearer policy and [Permissions](permissions.md) before launch; bodycam.use alone does not grant bodycam.record or evidence access.

## Inventory eligibility

Inventory eligibility is disabled by default, and the shipped adapter intentionally returns false until it is connected to your inventory system.

To enable it:

1. Set `Config.Wearers.Inventory.Enabled` to true.
2. Set `Item` to the exact server-side inventory item name.
3. Edit eventsBodycam/integrations/inventory.lua so `HasBodycamItem` checks the trusted server inventory for the supplied player source.
4. Select `any` or `all` according to your policy.
5. Test adding, removing, and transferring the item.

Do not accept a client event or client-supplied item state as proof of eligibility.

## EUP eligibility

EUP rules match a component slot, one or more drawable IDs, and optional texture IDs on the server-observed player ped. An empty texture table accepts every texture for an allowed drawable.

Example structure:

~~~lua
Config.Wearers.EUP = {
    Enabled = true,
    Rules = {
        {
            Component = 9,
            Drawables = { [1] = true },
            Textures = {}
        }
    }
}
~~~

Replace the example component and drawable IDs with the values used by your EUP pack. Component IDs must be between 0 and 11. At least one rule and one drawable are required when EUP eligibility is enabled.

The default server adapter uses server variation natives when available. If your runtime cannot provide the variation, edit eventsBodycam/integrations/eup.lua to query your trusted clothing resource server-side. An unavailable or failing EUP adapter denies the EUP check.

## Choose a policy

Common configurations include:

* `permission`: trusted department members can use Bodycam regardless of uniform or inventory.
* `any` with EUP enabled: a bodycam uniform component can qualify a wearer even without bodycam.use.
* `all` with inventory and EUP enabled: the player needs bodycam.use, the configured inventory item, and an approved uniform component.

`all` is the strictest option but depends on every enabled adapter being available. Test resource restarts and clothing/inventory failures so the policy denies access predictably.

## Test the policy

After restarting Events Core and Bodycam:

1. Test an authorized on-duty wearer and start a recording.
2. Test an off-duty player when RequireOnDuty is enabled.
3. Test an unauthorized player with no inventory item or approved EUP component.
4. If inventory is enabled, add and remove the configured item.
5. If EUP is enabled, test every allowed drawable and texture plus one disallowed variation.
6. Change ped, uniform, duty state, or routing bucket and confirm Bodycam re-evaluates the binding.

Eligibility loss, respawn, ped replacement, disconnect, and routing changes are reconciled server-side. Active sessions are finalized when the camera binding is removed, while completed evidence remains available according to its permissions and retention policy.
