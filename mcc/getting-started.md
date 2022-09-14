---
title: Mobile Command Center - Getting Started
description: This page will walk you through getting and installing the script.
published: true
date: 2022-09-14T22:53:20.976Z
tags: 
editor: markdown
dateCreated: 2022-08-29T17:42:49.311Z
---

# Getting Started

## Acquire the Script

After purchasing the script through the sonoran store you may [download the script through the keymaster account](/tebex-assets) that purchased the script. Upon downloading extract the file to a safe place.

## Install the Script
1. Inside the script package you just extracted will be two folders. Copy both to a folder in your server's resources folder called `[sonoranscripts]` note the `[]` in the name, without them it will not work.
![directory-example.png](/mcc/directory-example.png)
2. In the `sonoran-mcc` folder there will be a folder called `config`, once inside that folder you should see a file called  `config.CHANGEME.lua` you should rename that to be `config.lua` and configure the settings inside as you would like them to be configured based on the configuration documentation below.

3. Finally, in your `server.cfg` add the following:
```
ensure sonoran-mcc

add_ace resource.sonoran-mcc command allow
add_ace resource.sonoran-mcc_helper command allow
```

## Configuring The Script
Default `config.lua`: 
```lua
config = {}
config.configuration_version = 1.1 -- Do NOT change unless updating whole config file. Used by updater to tell you when new config file options are available.
config.auto_update = true -- Toggle Auto Updater, requires ace permissions to function. See Install Docs: https://docs.sonoran.store
config.debug_mode = false
config.eBrakeWithSliders = true -- Should the vehicle activate the "E BRAKE" when the sliders are out
config.keys = {
    -- Use https://docs.fivem.net/docs/game-references/controls/#controls to find the name...
    -- and use https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/keyboard/ to find index parameters for the key options below...
    cameraToggle = {cmd = 'camera', name = 'INPUT_PICKUP', indexParam = 'e', lang = 'Access MCC Cameras'}, -- Key to access cameras
    interiorLightToggle = {cmd = 'intlights', name = 'INPUT_REPLAY_CYCLEMARKERRIGHT', indexParam = 'RBRACKET', lang = 'Toggle MCC Interior Lights'}, -- Key to toggle rear interior lighting
    radioRepeaterToggle = {cmd = 'mccradio', name = 'INPUT_REPLAY_CYCLEMARKERLEFT', indexParam = 'LBRACKET', lang = 'Toggle MCC Radio Repeater'}, -- Key to toggle Sonoran Radio repeater
    menuToggle = {name = 'INPUT_SCRIPTED_FLY_ZUP', indexParam = 'PAGEUP'} -- Keybind to open the door control menu, can be changed in FiveM settings
}

-- Optional Menu/Commands/Keybinds controls for doors
config.doorControl = {
    method = 4, -- What method to use to control the doors | INTEGER | Options: 1 = commands, 2 = menu, 3 = none, 4 = commands & menu
    commands = {
        baseCmd = {cmd = 'mcctoggle', lang = 'Base MCC Toggle Command'}, -- Base command, see below for options
        antenna = {cmd = 'antenna', lang = 'Toggle MCC Antenna'}, -- toggle the antenna
        sliders = {cmd = 'sliders', lang = 'Toggle MCC Slide Out Sections'}, -- toggle the sliders
        rearDoor = {cmd = 'reardoor', lang = 'Toggle MCC Rear Door'}, -- toggle the read door
        frontLeft = {cmd = 'frontleft', lang = "Toggle MCC Driver's Door"}, -- toggle the front left door
        frontRight = {cmd = 'frontright', lang = "Toggle Passenger's Door"}, -- toggle the front right door
        allDoors = {cmd = 'all', lang = 'Toggle ALL MCC Doors'}, -- toggle all doors
        menuToggle = {cmd = 'togglem', lang = 'Toggle MCC Control Menu'} -- command to open the door control menu
    }
}

-- Translate the resource by customizing the messages below...
config.language = {
    mccHelpMessage = 'Press \n~' .. config.keys.cameraToggle.name .. '~ to access cameras \n~' .. config.keys.interiorLightToggle.name .. '~ to toggle interior lights',
    mccRadioHelpMessage = '\n~' .. config.keys.radioRepeaterToggle.name .. '~ to toggle mobile radio repeater',
    mccMenuHelpMessage = '\n~' .. config.keys.menuToggle.name .. '~ for more controls',
    radioRepeaterOn = 'MCC Radio repeater has been toggled on',
    radioRepeaterOff = 'MCC Radio repeater has been toggled off',
    notInMCC = 'You must be in the MCC to use this!',
    noValidArgument = 'You must provide a valid argument!',
    subCommand = 'Subcommand',
    subCommandOptions = 'Available options: ',
    missingSonoranRadio = 'Sonoran Radio not running, feature unavailable...',
    antennaNotUp = 'You must raise the antenna first!\n Use "/mcctoggle antenna" do to so',
    slidersOut = 'The MCC sliders are moving out... E brake will now engage.',
    slidersIn = 'The MCC sliders are now in... E brake will now disengange'
}
```

> There is no permission configuration required for this resource.
{.is-info}

## Config Breakdown
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `configuration_version` | This is used by the auto updater - simply don't touch it  | `Integer` |
| `auto_update` | Would you like the script to automatically update to the newest version? | `True` or `False`
| `debug_mode` | Would you like to enable debug prints? | `True` or `False`
| `eBrakeWithSliders` | Would you like to enable/ disable the MCC's emergency brake when sliders are opened/ closed | `True` or `False`

### `config.keys` Breakdown
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `cmd` | The command that will be triggered | `String`
| `name` | The key that will be displayed in the help menu upon entering the MCC | `String` [See Here for options](https://docs.fivem.net/docs/game-references/controls/#controls)
| `indexParam` | The default key that will be used to trigger said command | `Key` [See Here for options](https://docs.fivem.net/docs/game-references/input-mapper-parameter-ids/keyboard/)
| `lang` | The description of what the key will do | `String`

### `config.doorControl` Breakdown
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `method` | How would you like to control the MCC doors? | `1` = commands - `2` = menu `3` = none `4` = commands & menu
| `cmd` | The command that will be triggered | `String`
| `lang` | The description of what the command will do | `String`

### `config.language` Breakdown
Here you can imput your own translations or change the wording of all text prompts and popups.

## Commands 
These are the default names of commands, they may have been modified by the server owner.
| Command Name | Command Description  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/mcctoggle [door]` | This is the base command that. (See [`Available Doors`](https://docs.sonoran.store/en/mcc/getting-started#available-doors) for all the possible doors)
| `/togglem` | Toggle the MCC control menu
| `/mcccamera` | Access the MCC camera system
| `/intlights` | Toggle the MCC interior lights
| `/mccradio` | Toggle the MCC radio repeater

## Available Doors
| Name | Command  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| Antenna | `antenna`
| Sliders | `sliders`
| Rear Door | `reardoor`
| Driver's Door | `frontleft`
| Passenger's Door | `frontright`
| All Doors | `all`