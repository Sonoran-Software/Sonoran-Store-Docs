---
title: Evidence Camera - Getting Started
description: This page will walk you through getting and installing the script.
published: true
date: 2022-07-06T21:52:03.851Z
tags: 
editor: markdown
dateCreated: 2022-07-06T21:52:03.851Z
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

### General
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `command` | The command to toggle the camera | `string`
| `use_prop` | Use the camera prop | `true` or `false`
| `use_flash` | Make the camera prop flash | `true` or `false`

### Discord 
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `use_discord` | Send webhooks to Discord | `true` or `false` 
| `internal_log_url` | Webhook URL to send internal logs to | `url`
| `public_log_url` | Webhook URL to send public logs to | `url`

### Frameworks 
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `use_esx` | Use the ESX framework | `true` or `false` 
| `use_qbcore` | Use the QBCore framework | `true` or `false` 

### CAD 
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `use_cad` | Link with Sonoran CAD to send photos | `true` or `false` 
