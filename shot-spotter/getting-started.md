---
title: Shot Spotter - Getting Started
description: This page will walk you through getting and installing the script.
published: true
date: 2022-04-29T19:11:55.034Z
tags: 
editor: markdown
dateCreated: 2022-03-24T01:46:37.738Z
---

# Getting Started
## Acquire the Script
After purchasing the script through the sonoran store you may [download the script through the keymaster account](/tebex-assets) that purchased the script. Upon downloading extract the file to a safe place.

## Install the Script
1. Inside the script package you just extracted will be two folders. Copy both to a folder in your server's resources folder called `[sonoranscripts]` note the `[]` in the name, without them it will not work.
![directory-example.png](/shot-spotter/directory-example.png)
2. In the `sonoran-shotspotter/config` folder there will be a file called `config.CHANGEME.lua` you should rename that to be config.lua and configure the settings inside as you would like them to be configured based on the configuration documentation below. In that same folder will also be a file called `spotters.CHANGEME.json` which you should rename to `spotters.json` and use to manually place cameras based on the existing template, note you can also use the gun placement system in game.
![files-example1.png](/shot-spotter/files-example1.png)
![files-example2.png](/shot-spotter/files-example2.png)
3. Finally, in your `server.cfg` add the following:
> **NEVER** add `ensure sonoran-shotspotter_helper` or `ensure [sonoranscripts]` to your server.cfg as this will lead to crashing under specific conditions. {.is-warning}
```
ensure sonoran-shotspotter

add_ace resource.sonoran-shotspotter command allow
add_ace resource.sonoran-shotspotter_helper command allow
```


> Congrats! You have successfully installed Sonoran Shot Spotter. See Section II for configuration.
{.is-success}


## Configuration
### Permissions
Sonoran Shot Spotter contains highly configurable permissions that have a fit for any server. In this section you will see options for standalone, QB-Core and ESX. Below will detail every option in this section, it's meaning and options.  

| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `restricted` | Would you like to restrict the `/shotspot` command or allow anyone to use it? | `true` or `false` |
| `use_ace_perms` | Would you like to utilize FiveM Ace Permissions? | `true` or `false`
| `ace_object` | The ace name that will be used to assign permissions. More can be read about Ace Permissions [Here](https://forum.cfx.re/t/basic-aces-principals-overview-guide/90917)  | `string`
| `use_esx` | Would you like to utilize ESX framework jobs for permissions? | `true` or `false`
| `police_job` | What is the name of your ESX jobs that should have access to the shotspotter? | `array`
| `use_qbcore` | Would you like to utilize QB-Core framework jobs for permissions? | `true` or `false`
| `police_job` | What is the name of your QB-Core jobs that should have access to the shotspotter? | `array`

### Webhooks
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `webhook_URL` | The Discord URL to send webhooks to | `string` 
| `onduty` | Would you like to send a webhook when someone goes on duty? | `true` or `false` 
| `offduty` | Would you like to send a webhook when someone goes off duty? | `true` or `false`
| `onshot` | Would you like to send a webhook when the shot spotter is triggered? | `true` or `false`

### General 
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `use_cad` | Would you like to utilize the Sonoran Cad framework to automatically send 911 calls to Sonoran Cad? | `true` or `false`
| `use_command` | Would you like to be able to toggle the shot spotter alerts in game on and off? | `true` or `false`
| `shotspot_cmd` | The command to toggle the shot spotter alerts in game | `string`
| `show_on_map` | Would you like to show a blip on the map when the shot spotter was triggered? | `true` or `false`
| `radius` | How large of a radius should each shot spotter scan? | `integer` 

### Spotter
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `use_object` | Would you like to utilize the included shot spotter object? | `true` or `false`
| `cooldown` | How long of a cooldown should be between shot spotter alerts?  | `integer` (time in seconds)

### Whitelist
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `peds` | Peds that will not trigger the shot spotter | `array` 
| `weapons` | Weapons that will not trigger the shot spotter | `array`

### Lang
The language section was created to make the script as universal as possible. Please simply edit the strings to your prefered text. 
#### Note: Available place holders are {{street}}, {{spotter}} and {{player}}
> Congrats! You have successfully configured Sonoran Shot Spotter. See Section III for spotter locations.
{.is-success}

## Spotter Locations 
These locations can be found and set in the `spotters.json` found within the `config` folder. To configure spotters follow this chart:

| Config Option          | Option Description                                                                                                                         | Possible Values    | Notes | 
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|------------------------|
| `ID` | The shot spotter ID (used internally) | `integer` | This cannot be duplicated
| `x`, `y`, `z` | The in game coordinates | `integer` | N/A
| `pitch`, `roll`, `yaw` | Rotation values | `integer` | N/A

## Commands
| Command Name          | Command Description                                                                                                                         | Required Permission    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `/shotspot` | This command will toggle the user's shot spotter status, either enabling or disabling shot spotter alerts and blips | LEO or as configured |
| `/showspotterid` | Show the ID above the shot spotters | Admin
| `/showspotterpos` | Show the position of the shot spotters | Admin
| `/changepositiondata` | Change the position data of the shot spotter | Admin
| `/reloadspotters` | Reload all spotters and positions | Admin
| `/spawnnewspotter` | Activate the placement gun | Admin