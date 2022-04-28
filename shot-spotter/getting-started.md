---
title: Getting Started
description: This page will walk you through getting and installing the script.
published: false
date: 2022-04-28T03:00:04.975Z
tags: 
editor: markdown
dateCreated: 2022-03-24T01:46:37.738Z
---

# Sonoran Shot Spotter Written Installation

## The Basics

Step 1.) Extract your downloaded folder into your server files
> This can be done using [WinRAR](https://www.win-rar.com/predownload.html?&L=0) {.is-info}

Step 2.) Change the folder name to `sonoran_shotspotter`
> This name **MUST** match exactly {.is-warning}

Step 3.)  Add the following line to your `server.cfg` (or other startup config): 
```
ensure sonoran_shotspotter
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