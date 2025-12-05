---
title: Radar Display - Getting Started
published: true
date: 2023-09-14T22:48:07.134Z
tags: null
editor: markdown
dateCreated: 2022-10-24T17:52:19.625Z
description: >-
  This page will walk you through getting and installing the Radar Display
  script.
---

# Getting Started

## Acquire the Script

After purchasing the script through the Sonoran store you may [download the script through the keymaster account](../general/tebex-assets.md) that purchased the script. Upon downloading extract the file to a safe place.

## Install Prerequisite

{% hint style="warning" %}
You must use the Sonoran Fork of WK\_Wars2x found [here](https://github.com/Sonoran-Software/wk_wars2x/releases/latest).

Minimum version required: `v1.3.3-sonoran`
{% endhint %}

{% hint style="info" %}
If you are using the Sonoran CAD FiveM Integration, **DO NOT** install a second copy of WK\_Wars2x, it is included and updated with the `sonorancad` resource.
{% endhint %}

WK\_Wars2x Radar script is required for this resource to function fully. Please ensure you are using the **Sonoran Fork** of WK\_Wars2x. This is included with the [Sonoran CAD FiveM Integration](https://info.sonorancad.com/integration-plugins/integration-plugins/framework-installation) by default.

You can also download the latest release of the resource [here](https://github.com/Sonoran-Software/wk_wars2x/releases/latest).

## Install the Script

1.  Inside the script package you just extracted will be two folders. Copy both to a folder in your server's resources folder called `[sonoranscripts]` note the `[]` in the name, without them it will not work.&#x20;

    <figure><img src="../.gitbook/assets/directory_example (1).png" alt=""><figcaption><p>Sonoran Software - Radar Display - Folders</p></figcaption></figure>
2. In the `sonoran-radar/config` folder there will be a file called `config.CHANGEME.lua` you should rename that to be `config.lua` and configure the settings inside as you would like them to be configured based on the configuration documentation below. In that same folder will also be a file called `radars.CHANGEME.json` which you should rename to `radars.json` and use to manually place cameras based on the existing template, note you can also use the menu placement system in game.
3. Finally, in your `server.cfg` add the following:

```
ensure sonoran-radar

add_ace resource.sonoran-radar command allow
add_ace resource.sonoran-radar_helper command allow
```

## Configuring the Script

<details>

<summary>Default <code>config.lua</code></summary>

```lua
Config = {
    debug_mode = true,
    cars = {
        addonCars = {
            ['19tahoec3'] = {
                active = false,
                name = '19tahoec3',
                redneckCar = true
            }
        },
        blacklistedCars = {
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
        },
        bones = {'chassis'}
    },
    update_speeds = {
        patrol_speed = 100, -- Time in MS between each update for the patrol speed
        target_speed = 250, -- Time in MS between each update for the target speed
        lock_speed = 250 -- Time in MS between each update for the lock speed
    },
    stalker = {
        default_ant = 'front' -- The default antenna to pull data from | Options: 'front' or 'back'
    },
    commands = {
        addNewRadar = 'addnewradar', -- The command to add a new radar
        restricted = true, -- restrict this command - you want this
        allowedToPlace = 'radar.admin' -- Ace group allowed to place the radar
    },
    lang = {
        addNewRadarHelp = 'Open the menu to begin spawning a new radar model',
        notInEmergency = 'You must be in a Emergency vehicle to use this!',
        vehNotCompatible = 'This vehicle is not compatible with the radar placement system!'
    }
}
```

</details>

## Radar Location Config

You have two options for placing new radars:

1. You can use the command `/addnewradar` while in a LEO vehicle to initiate spawning a new radar and generate the relevant config data
   * After running this command you will be prompted by a menu that will take you through the spawning process.
   * You may need to modify some of the rotation values manually to get that perfect placement you are looking for.
2. You can manually copy and paste an existing config and then modify the values to meet your needs for the new camera

### `radars.json` Property Explanation

| Property Name | Example | Notes                                                                                                                                                                                 |
| ------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ID`          | `2`     | `ID` must be unique. No other radar can share this ID                                                                                                                                 |
| `Position`    |         | This is a table that contains the x, y, and z coords of the radar                                                                                                                     |
| `Rotation`    |         | This is a table that contains the x, y, and z rotation of the radar                                                                                                                   |
| `Vehicle`     | `FBI`   | This is the vehicles spawn code                                                                                                                                                       |
| `Bone`        | `-1`    | This is the index of the bone you would like to attach the radar to                                                                                                                   |
| `type`        | `1`     | <p>Enum:</p><p><code>1</code> Radar model with front and rear antennas</p><p><code>2</code> Radar model with only front antenna</p><p><code>3</code> Radar model with no antennas</p> |

## Commands

| Command Name   | Command Description                                                 | Required Permission    |
| -------------- | ------------------------------------------------------------------- | ---------------------- |
| `/addnewradar` | This command will initiate the radar spawning and attaching process | Admin or as configured |

## Default Vehicles

| Vehicle Spawncode | Adds Radar by Default       |
| ----------------- | --------------------------- |
| `FBI`             | `yes`                       |
| `FBI2`            | `yes`                       |
| `POLICE`          | `yes`                       |
| `POLICE2`         | `yes`                       |
| `POLICE3`         | `BUGGED`(Investigating fix) |
| `POLICE4`         | `yes`                       |
| `POLICEOLD1`      | `no`                        |
| `POLICEOLD2`      | `no`                        |
| `SHERIFF`         | `yes`                       |
| `SHERIFF2`        | `yes`                       |
