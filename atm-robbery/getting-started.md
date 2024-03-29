---
title: ATM Robbery - Getting Started
published: true
date: 2023-05-15T21:41:14.921Z
tags: null
editor: markdown
dateCreated: 2022-12-22T00:07:47.214Z
description: This page will walk you through getting and installing the ATM Robbery script.
---

# Getting Started

## Acquire the Script

After purchasing the script through the sonoran store you may [download the script through the keymaster account](../general/tebex-assets.md) that purchased the script. Upon downloading extract the file to a safe place.

## Install the Script

1.  Inside the script package you just extracted will be two folders. Copy both to a folder in your server's resources folder called `[sonoranscripts]` note the `[]` in the name, without them it will not work.&#x20;

    <figure><img src="atm_file.png" alt=""><figcaption><p>Sonoran Software - ATM Robbery - Folders</p></figcaption></figure>
2. In the `sonoran-atmrobbery/config` folder there will be a file called `config.CHANGEME.lua` you should rename that to be `config.lua` and configure the settings inside as you would like them to be configured based on the configuration documentation below. In that same folder will also be a file called `atms.CHANGEME.json` which is a storage file for all ATMs placed using the placement gun. Change the name to `atms.json`. You can also edit the `discord.CHANGEME.lua` file to add discord webhook updates for when the state of the ATM changes. Change the name to `discord.lua`.

{% hint style="danger" %}
Only ever change the name of the `atms.json` **ON INSTALL**, not on subsequent updates!
{% endhint %}

![Sonoran Software - ATM Robbery - Config folder](../evidence-camera/config-folder.png)

{% hint style="warning" %}
**QBCore** Specific - Please follow `Step 3` to ensure inventory items have correct photos!
{% endhint %}

3. Drag the `jackhammer.png` and `rope.png` from `\sonoran-atmrobbery\config` into the following folder: `\resources\[qb]\qb-inventory\html\images`

{% hint style="warning" %}
**ESX Specific** Specific - Please follow `Step 4` to ensure inventory items work!
{% endhint %}

4.  a.) Import the `ESX Installme.sql` file into your ESX database

    _**If NOT using**** ****`ox_inventory`**** ****continue to step 5**_

    b.) Add the following code to the `/ox_inventory/data/items.lua` file

    ```lua
    	['sonoran_jackhammer'] = {
     	label = 'Jackhammer',
     	weight = 1,
     	stack = true,
     	close = true,
     	description = nil
     },
     ['sonoran_rope'] = {
     	label = 'Rope',
     	weight = 1,
     	stack = true,
     	close = true,
     	description = nil,
     },
    ```
5. Finally, in your `server.cfg` add the following:

{% hint style="warning" %}
**NEVER** add `ensure sonoran-atmrobbery_helper` or `ensure [sonoranscripts]` to your server.cfg as this will lead to crashing under specific conditions.
{% endhint %}

```
ensure sonoran-atmrobbery

add_ace resource.sonoran-atmrobbery command allow
add_ace resource.sonoran-atmrobbery_helper command allow
```

## Configuring the Script

{% hint style="danger" %}
This script utilizes two separate Lua configs. This is for security reasons. Having a separate config for your Discord Configuration keeps them server side only and safer from malicious clients.
{% endhint %}

<details>

<summary>Default <code>config.lua</code></summary>

```lua
Config = {}
-- General Configuration Section --
Config.configuration_version = 1.1
Config.debug_mode = false -- Only useful for developers and if support asks you to enable it
Config.permissionMode = "ace" -- Available Options: ace, framework, custom
Config.auto_config = false

-- Ace Permissions Section --
Config.acePerms = {
    aceObjectPlace = "sonoran.atms", -- Select the ace for placing new ATM's and using admin repair
    aceObjectNotifications = "sonoran.police", -- Select the ace for receiving in-game notifications
    aceObjectCanSteal = "sonoran.theif", -- Select the ace for being allowed to steal ATM's
}

-- Framework Related Settings --
Config.framework = {
    frameworkType = "qb-core", -- This setting controls which framework is in use options are esx or qb-core
    policeJobNames = {"police"}, -- An array of job names that should receive notifications
    inventoryType = "normal", -- Which inventory you would like to use normal, quasar, ox_inventory (OX Will only work for ESX Legacy as of now)
    allowedToPlaceGroups = {"admin"}, -- The permission group that should be allowed to place new systems
    theifJobNames = {"unemployed"}, -- An array of job names that should receive notification_message
    useTheifJobListAsBlacklist = false, -- This will treat the theif job list as a blacklist rather than a whitelist
    reward = {
        amountRange = {10000, 20000}, -- The range of money they can recieve
        rewardType = "cash", -- The type of money they will recieve
        rewardItems = false,
        Items = {
            {item= "ITEM_NAME", amount = 3},
        }
    },
}

-- Configuration For Custom Permissions Handling --
Config.custom = {
    checkPermsServerSide = true, -- If true the permission event will be sent out to the server side resource, this is recommended
    permissionCheck = function(_, type) -- This function will always be called server side.
        if type == 0 then -- Check for admin
            return true or false -- Return true if they have admin, return false if they don't
        elseif type == 1 then -- Check for notification perms
            return true or false -- Return true if they have permissions, return false if they don't
        elseif type == 2 then -- Check for hacker perms
            return true or false -- Return true if they have permissions, return false if they don't
        elseif type == 3 then -- Check for repair perms
            return true or false -- Return true if they have permissions, return false if they don't
        end
    end
}

Config.atmModels = {
    ['old'] = -870868698,
    ['blue'] = -1126237515,
    ['red'] = -1364697528,
    ['green'] = 506770882
}

-- Choose Custom Command Names --
Config.commands = {
    placementGun = 'spawnnewatm', -- Command used to start the placement gun
    -- addWinch = 'addwinch', -- IN DEVELOPMENT
    -- removeWinch = 'removewinch', -- IN DEVELOPMENT
    withRope = 'attachrope', -- Command used to steal ATMs with rope
    drillATM = 'drillatm', -- Command used to initiate the final drilling into an atm
}

-- Feature Settings That Don't Require Other Resources --
Config.standaloneFeatures = {
    showNotificationBlipsForPolice = true, -- Should police see a blip when a car is pinged?
    blipsExpireAfterSeconds = 90, -- Number of seconds before the blip type above is removed
    enable_auto_update = true, -- Should the script automatically update itself, it will check for updates regardless
    delayForNotifyingStealing = 10000, -- Time in miliseconds between a successful hack and notification going out, set to 0 to disable delay
    distToDrill = 10, -- The distance a player must be from the inital location of the ATM robbery to begin drilling into the atm
}

--[[
    Placeholder list:
    {{POSTAL}} = Postal
    {{EVENT_TYPE}} = Event Type
    {{ATM_NAME}} = ATM Name
]]


-- Settings For Integrations With Other Resources --
Config.integration = {
    SonoranCAD_integration = {
        use = true, -- Should any of the options below be used? Integration with this script requires at least a Plus subscription.
        addLiveMapBlips = true, -- Should blips for the power systems be added to the live map? This requires the Pro SonoranCAD plan
        enable911Calls = true, -- Should 911 calls be generated in the CAD when a BOLO vehicle or speeder is detected?
        ["911_caller"] = "Automated ATM Alerts", -- Who should the 911 call appear to be from?
        ["911_message"] = "{{EVENT_TYPE}} Alert at ATM with name {{ATM_NAME}}", -- Configurable 911 call description
        nearestPostalPlugin = "nearest-postal", -- If you want to use postals, what is the exact name of your postals script?
        disableInGameWithDispatch = false, -- If true disables in-game notifications when dispatch is online in CAD
        disableCadWithoutDispatch = false -- If true disables CAD notifications when dispatch is offline in CAD
    }
}

-- Notification Settings --
Config.notifications = {
    type = "native", -- Available options: native, pNotify, okokNotify, or cadonly
    notificationTitle = "{{EVENT_TYPE}}", -- Notification Title for methods that support it
    -- Uncomment line below and comment line 105 if you plan to use pNotify
    -- notificationMessage = "<b>{{EVENT_TYPE}} Alert</b></br>Alert at ATM with name {{ATM_NAME}}"
    notificationMessage = "{{EVENT_TYPE}} Alert\nAlert at ATM with name {{ATM_NAME}}", -- The text of the notification
}

-- Vehicle Settings --
Config.vehicles = {
    ['DLOADER'] = {
        trunk = true,
        pull = true,
    },
    ['OPENWHEEL2'] = {
        trunk = false,
        pull = false,
    }
}

-- Translation Settings --
Config.lang = {
    general = {
        drillAtm = 'Press ~g~E ~w~to begin stealing the ATM',
        tooHot = 'The drill got too ~r~hot~w~ and broke!\nATM robbery ~r~failed~w~!',
        robCanceled = 'The ATM robbery was ~r~canceled~w~.',
        drillSuccess = 'The ATM has been successfully drilled into.\nReady for winch',
        inVeh = 'You get out of the vehicle to do this!',
        -- alrWinch = 'This vehicle already has a winch',
        -- winchAttach = 'The winch has been attached to this vehicle!',
        -- winchDetached = 'The winch has been detached from this vehicle!',
        -- noWinchDetected = 'This vehicle does not have a winch!',
        noVehNearby = 'There is no vehicle nearby!',
        noAtmNearby = 'There is no ATM nearby!',
        noAttachedAtm = 'There is no ATM attached!',
        loadAtm = 'Press ~g~H ~w~to load the ATM into your vehicle',
        unloadAtm = 'Press ~g~H ~w~to unload the ATM from your vehicle'
    },
    commands = {
        alrSpawning = '[Err] You are already spawning an ATM. Please cancel current spawn first!',
        atmPlaced = 'ATM Placed!',
        spawnNew = 'Spawn a new ATM and save it to the config based on pointing a gun',
        labelText = 'The label you want to apply to the shot spotter',
        atmTypes = 'The type of ATM model to spawn. (old, blue, red, green)',
        -- addWinch = 'Add a winch to your nearby vehicle', -- IN DEVELOPMENT
        -- removeWinch = 'Remove a winch to your nearby vehicle', -- IN DEVELOPMENT
        withRope = 'Use a rope to steal the ATM',
        drillATM = 'Initiate the final drilling into the ATM'

    },
    cadCall = {
        callDesc = 'This is an automated call from an ATM alarm',
        callerName = 'Automated ATM Alarm'
    }
}

```

</details>

<details>

<summary>Default <code>discord.lua</code></summary>

```lua
DiscordConfig = {
    enabled = false, -- Should discord webhooks be used?
    webhook_url = '', -- See https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks
    webhook_title = '{{EVENT_TYPE}} Alert', -- The title of the webhook embed
    webhook_message = 'An ATM on {{STREET}} ({{POSTAL}}) is currently being stolen' -- The message that the webhook displays
}
```

</details>

## Setting Up Permissions (Ace Permissions Only)

To use this script you must assign the permissions object to the groups which you would like to have permissions. You can find the objects needed in the config under `Config.ace_perms` section in the config. To assign these perms you must add the lines to your `server.cfg` or where ever you setup permissions on your server. You can learn more about Ace Permissions [here](https://forum.cfx.re/t/basic-aces-principals-overview-guide/90917).

Example:

```lua
add_ace group.admin sonoran.atms allow
add_ace group.leo sonoran.police allow
add_ace group.hacker sonoran.theif allow
```

## ATM Location Config

You have two options for placing new ATMs:

1. You can use the command `/spawnnewatm [model] [label]` to initiate spawning a new ATM and generate the relevant config data
   * After running this command you must pull out, aim, and shoot with a gun to confirm placement.
   * You may need to modify some of the rotation values manually to get that perfect placement you are looking for.
   * [Click here for more information.](../general/gun-placement.md)
2. You can manually copy and paste an existing config and then modify the values to meet your needs for the new ATM

### `atms.json` Property Explanation

<table><thead><tr><th width="189">Property Name</th><th width="110.33333333333331">Example</th><th>Notes</th></tr></thead><tbody><tr><td><code>ID</code></td><td>2</td><td><code>ID</code> must be unique. No other ATM can share this ID</td></tr><tr><td><code>Type</code></td><td><code>green</code></td><td>Valid values are <code>old</code> and <code>blue</code>, <code>green</code>, <code>red</code></td></tr><tr><td><code>Position</code></td><td></td><td>This is a table that contains the x, y, and z coords of the ATM</td></tr><tr><td><code>Rotation</code></td><td></td><td>This is a table that contains the x, y, and z rotation of the ATM</td></tr><tr><td><code>Label</code></td><td><code>Test 1</code></td><td>This is a human readable label for the ATM, can have spaces</td></tr></tbody></table>

These are the default names of commands, they may have been modified by the server owner.

### Commands

<table><thead><tr><th width="199">Command Name</th><th width="325.3333333333333">Command Description</th><th>Required Permission</th></tr></thead><tbody><tr><td><code>/spawnnewatm  [model] [label]</code></td><td>This command will allow an admin to spawn a new ATM using a gun placement system where the name argument is the label for the new ATM and the prop argument is the text name of the model to use</td><td>Admin or as configured</td></tr><tr><td><code>/attachrope</code></td><td>This command is used to attach the rope between the ATM and your vehicle</td><td>Everyone or as configured</td></tr><tr><td><code>/drillatm</code></td><td>This command will initiate the final drilling into the ATM</td><td>Everyone or as configured</td></tr></tbody></table>
