---
title: Shot Spotter Installation
description: 
published: false
date: 2022-04-04T00:44:49.739Z
tags: 
editor: markdown
dateCreated: 2022-03-24T01:48:28.702Z
---

# Sonoran Shot Spotter


## Written Installation

### The Basics

Step 1.) Extract your downloaded folder into your server files
> This can be done using [WinRAR](https://www.win-rar.com/predownload.html?&L=0) {.is-info}

Step 2.) Change the folder name to `sonoran_shotspotter`
> This name **MUST** match exactly {.is-warning}

Step 3.)  Add the following line to your `server.cfg` (or other startup config): 
```
ensure sonoran_shotspotter
```

Congrats! You have successfully installed Sonoran Shot Spotter. See Section II for configuration.

### Configuration
#### Permissions
Sonoran Shot Spotter contains highly configurable permissions that have a fit for any server. In this section you will see options for standalone, QB-Core and ESX. Below will detail every option in this section, it's meaning and options.  

| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `restricted` | Would you like to restrict the `/shotspot` command or allow anyone to use it? | `true` or `false` |
| `use_ace_perms` | Would you like to utilize FiveM Ace Permissions? | `true` or `false`
| `ace_object` | The ace name that will be used to assign permissions. More can be read about Ace Permissions [Here](https://forum.cfx.re/t/basic-aces-principals-overview-guide/90917)  | `string`
| `use_esx` | Would you like to utilize ESX framework jobs for permissions? | `true` or `false`
| `police_job` | What is the name of your ESX jobs that should have access to the shotspotter? | `arary`
| `use_qbcore` | Would you like to utilize QB-Core framework jobs for permissions? | `true` or `false`
| `police_job` | What is the name of your QB-Core jobs that should have access to the shotspotter? | `arary`

#### Webhooks
| Config Option          | Option Description                                                                                                                         | Possible Values    |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `webhook_URL` | The Discord URL to send webhooks to | `string` 
| `onduty` | Would you like to send a webhook when someone goes on duty? | `true` or `false` 
| `offduty` | Would you like to send a webhook when someone goes off duty? | `true` or `false`
| `onshot` | Would you like to send a webhook when the shot spotter is triggered? | `true` or `false`

#### General 
