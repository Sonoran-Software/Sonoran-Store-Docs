---
title: Radar Detector/ Jammer  - Getting Started
description: This page will walk you through getting, installing and using the Radar Detector script.
published: false
date: 2023-06-05T22:19:28.303Z
tags: 
editor: markdown
dateCreated: 2023-06-05T22:05:11.281Z
---

# Getting Started

## Acquire the Script

After purchasing the script through the sonoran store you may [download the script through the keymaster account](/tebex-assets) that purchased the script. Upon downloading extract the file to a safe place.

## Install Prerequisite

> You must use the Sonoran Fork of WK_Wars2x found [here](https://github.com/Sonoran-Software/wk_wars2x/releases/latest).
>
> Minimum version required: `v1.3.4-sonoran`
{.is-warning}

> If you are using the Sonoran CAD FiveM Integration, **DO NOT** install a second copy of WK_Wars2x, it is included and updated with the `sonorancad` resource.
{.is-info}


WK_Wars2x Radar script is required for this resource to function fully. Please ensure you are using the **Sonoran Fork** of WK_Wars2x. This is included with the [Sonoran CAD FiveM Integration](https://info.sonorancad.com/integration-plugins/integration-plugins/framework-installation) by default.

You can also download the latest release of the resource [here](https://github.com/Sonoran-Software/wk_wars2x/releases/latest).

## Install the Script

1. Inside the script package you just extracted will be two folders. Copy both to a folder in your server's resources folder called `[sonoranscripts]` note the `[]` in the name, without them it will not work.
![rcypcj7x.png](/radar-detector/rcypcj7x.png)
2. In the `sonoran-radarddetector/config` folder there will be a file called `config.CHANGEME.lua` you should rename that to be `config.lua` and configure the settings inside as you would like them to be configured based on the configuration documentation below. In that same folder will also be a file called `detectors.CHANGEME.json` which you should rename to `detectors.json`.
3. Finally, in your `server.cfg` add the following:

```
ensure sonoran-radardetector

add_ace resource.sonoran-radardetector command allow
add_ace resource.sonoran-radardetector_helper command allow
```

## Configuring the Script
Default config.lua:
```lua
Config = {}

Config.debug_mode = true -- Enable debug mode
Config.configuration_version = 1.0
Config.auto_update = true -- Enable auto updates
Config.auto_config = false

Config.lang = {
    addNewRadarHelp = 'Open the menu to begin spawning a new radar detector model',
    vehNotCompatible = 'This vehicle is not compatible with the radar detector placement system!',
    vehAlrRadar = 'This vehicle already has a valid radar!',
    noItem = 'You do not have a radar detector in your inventory!'
}

Config.commands = {
    radarDetectorMenu = 'detectormenu',
    restricted = false -- should the detector menu be restricted?
}

Config.permissionMode = "ace" -- Available Options: ace, framework, custom

-- Ace Permissions Section --
Config.acePerms = {
    aceObjectUseMenu = "sonoran.radardetector", -- Select the ace for placing new ATM's and using admin repair
}

-- Framework Related Settings --
Config.framework = {
    frameworkType = "qb-core", -- This setting controls which framework is in use options are esx or qb-core
    inventoryType = "normal", -- Which inventory you would like to use normal, quasar, ox_inventory (OX Will only work for ESX Legacy as of now)
    civilianJobNames = {"unemployed"}, -- An array of job names that should be allowed to use the radar menu
    useCivilianJobListAsBlacklist = false, -- This will treat the theif job list as a blacklist rather than a whitelist
}

-- Configuration For Custom Permissions Handling --
Config.custom = {
    checkPermsServerSide = true, -- If true the permission event will be sent out to the server side resource, this is recommended
    permissionCheck = function(_, type) -- This function will always be called server side.
        if type == 0 then -- Check permission to use the menu
            return true or false -- Return true if they have admin, return false if they don't
        end
    end
}

Config.general = {
    playRandomSounds = true, -- Should random sounds be played when the radar is powered on?
    randomSounds = {"x_band", "k_band", "ka_band", "laser", "gps_connected"}, -- An array of sounds that should be played randomly when the radar is powered on
    randomSoundInterval = 5, -- How often in minutes should a random sound be played if a radar is on?
    detectDistance = 250.0, -- How far away should the player be able to detect a radar?
    notificationType = "native", -- Available options: native, pNotify, okokNotify
}

Config.blacklistedCars = {
        "FIRETRUK",
        "LGUARD",
        "PBUS",
        "POLMAV",
        "POLICET",
        "PRANGER",
        "PREDATOR",
        "RIOT",
        "RIOT2",
        "AMBULAN"
}
```
## Configuration Breakdown
| Config Option | Option Description | Default Value | Type | 
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|------------------------|
| `debug_mode` | Enable enhanced logging - typically for diagnosing issues | `false` | Boolean
| `configuration_version` | The version your config file is currently set to. **DO NOT EDIT** | `1.0` | Int
| `auto_update` | Enabled auto updates with our Sonoran Updater | `true` | Boolean
| `auto_config` | For internal use only, do not edit | `false` | Boolean
| `lang` | Here you will find our easy translations, simply edit if you would like to update the language used in the script | English | String
| `radarDetectorMenu` | Command to open the radar detector menu | `detectormenu` | String |
| `restricted` | Specifies if the detector menu should be restricted | `false` | Boolean |
| `permissionMode` | Permission mode for the radar detector | `ace` | String |
| `aceObjectUseMenu` | Select the ace premission allowed to use the radar detector menu | `sonoran.radardetector` | String |
| `frameworkType` | This setting controls which framework is in use options are esx or qb-core | `qb-core` | String |
| `inventoryType` | Which inventory you would like to use normal, quasar, ox_inventory (OX Will only work for ESX Legacy as of now) | `normal` | String |
| `civilianJobNames` | An array of job names that should be allowed to use the radar menu | `{"unemployed"}` | Table |
| `useCivilianJobListAsBlacklist` | This will treat the civilian job list as a blacklist rather than a whitelist | `false` | Boolean |
| `playRandomSounds` | Should random sounds be played when the radar is powered on? | `true` | Boolean |
| `randomSounds` | An array of sounds that should be played randomly when the radar is powered on | `{"x_band", "k_band", "ka_band", "laser", "gps_connected"}` | Table |
| `randomSoundInterval` | How often in minutes should a random sound be played if a radar is on? | `5` | Number |
| `detectDistance` | How far away should the player be able to detect a radar? | `250.0` | Number |
| `notificationType` | Available options: native, pNotify, okokNotify | `native` | String |
| `blacklistedCars` | Vehicles that are not allowed to have a radar detector put in them | `Table` | Table

## Commands
| Command Name | Command Description | Required Permission |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/detectormenu` | This command will open the radar detector/jammer menu and allow you to configure and spawn your radar detector/jammer | Everyone or as configured |

## Usage 
1. **Choose an eligible car**: Select a car that is compatible with the radar detector/jammer script.

2. **Use the command `/detectormenu`**: Open the chatbox or console and type `/detectormenu` to bring up the radar detector menu.

3. **Open the spawning menu**: In the radar detector menu, navigate to the "Spawning Menu" option and select it to open the spawning menu.

4. **Spawn the prop**: Within the spawning menu, choose the desired prop and spawn it into the game world.

5. **Navigate to the attaching menu**: Use the "Backspace" or the designated key to go back in the menu and access the "Attaching Menu".

6. **Attach the prop**: Select the "Attach" option in the attaching menu and use the on-screen controls to move the prop to the desired location on the vehicle.

7. **Confirm placement**: Once the prop is positioned correctly, press the "Confirm Placement" button to finalize the attachment.

8. **Access the radar detector remote menu**: Use the "Backspace" key or the designated key to go back in the menu and select the "Radar Detector Remote Menu".

9. **Toggle the jammer and detector**: Within the radar detector remote menu, adjust the settings to toggle the jammer and detector functionality according to your preferences.

10. **Turn on the power**: Enable the power switch in the radar detector remote menu to activate the radar detector/jammer system.

11. **Enjoy the protection from police radars**: With the radar detector/jammer active, you will now be protected from police radars while driving your vehicle.