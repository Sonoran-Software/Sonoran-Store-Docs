---
title: Mobile Command Center - Getting Started
description: This page will walk you through getting and installing the script.
published: false
date: 2022-09-08T00:59:01.695Z
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
3. Finally, in your `server.cfg` add the following:
```
ensure sonoran-mcc

add_ace resource.sonoran-mcc command allow
add_ace resource.sonoran-mcc_helper command allow
```
