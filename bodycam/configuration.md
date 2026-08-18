---
title: Configuration
published: true
date: 2026-08-18T00:00:00.000Z
tags: [fivem, bodycam, configuration, events core]
editor: markdown
dateCreated: 2026-08-18T00:00:00.000Z
description: Configure Bodycam branding, capture settings, triggers, wearer eligibility, retention, permissions, and integrations.
---

# Bodycam configuration

Bodycam ships with usable standalone defaults and requires the free Events Core package. Test configuration changes on a non-production server before using the system for evidence workflows.

## Configuration files

### Events Core

| File | Purpose |
| --- | --- |
| eventsCore/config/config.lua | Framework selection, recording limits, replay speeds, processing, retention, and audit settings |
| eventsCore/config/storage.lua | Local storage limits and optional allowed playback hosts |
| eventsCore/config/permissions.lua | Permission behavior and trusted processing resources |
| eventsCore/config/integrations.lua | Optional CAD and Discord settings |

### Bodycam

| File | Purpose |
| --- | --- |
| eventsBodycam/config/config.lua | Branding, capture range, timing, triggers, keys, integrations, and request limits |
| eventsBodycam/config/cameras.lua | Ped bone, camera offset, rotation offset, and field of view |
| eventsBodycam/config/wearers.lua | Eligibility mode, duty requirement, inventory item, and EUP rules |
| eventsBodycam/config/retention.lua | Retention days for each evidence classification |
| eventsBodycam/config/permissions.lua | Bodycam ACE nodes, framework job grades, and trusted resources |
| eventsBodycam/config/update.lua | Informational update-check toggle |
| eventsBodycam/integrations/ | Optional dispatch, inventory, EUP, phone, voice-metadata, CAD, Discord, and clip-provider bridges |

Keep API tokens, Discord webhooks, and private endpoints in server-only files. Never place credentials in client configuration or NUI files.

## Important shipped defaults

| Setting | Default |
| --- | --- |
| Capture radius | 85 metres around the wearer |
| Pre-event context | 60 seconds |
| Automatic post-event time | 20 seconds |
| Maximum manual recording | 1,800 seconds (30 minutes) |
| Replay perspectives | One recorded bodycam perspective |
| Bodycam bone | 24818, SKEL_Spine3 |
| Field of view | 78 degrees |
| Open terminal | F6 |
| Start or stop recording | F11 |
| Add evidence marker | No default key; /bodycammark is available |
| Trigger polling | Every 500 ms |
| Manual, gunshot, damage, officer-down, and panic triggers | Enabled |
| Damage threshold | 20 health points |
| Officer-down threshold | 110 health or dead |
| Trigger cooldown | 5 seconds |
| CAD and Discord actions | Disabled |
| Automatic update check | Disabled |

## Branding

`Config.Brand` controls the department name, product name, accent color, timezone label, and privacy disclosure. Update the disclosure to describe your server's actual evidence and privacy policy. The default workflow stores reconstructed gameplay state and voice/radio activity metadata, not native voice audio bytes.

## Framework selection

Events Core is standalone by default. Set its supported framework option to esx, qbcore, qbox, or automatic detection only when that framework is installed and available. Framework jobs can provide standard or elevated Bodycam actions using the grade tables in eventsBodycam/config/permissions.lua.

## Camera placement

The shipped camera uses the upper-spine bone. Custom peds and EUP packs can place that bone differently, so verify the recorded angle with every supported uniform and ped before launch. Adjust the bone, offset, rotation, and field of view in eventsBodycam/config/cameras.lua on a test server.

Server runtimes cannot always resolve exact animated bone coordinates. When that occurs, Events Core marks the recorded transform as approximate instead of presenting it as an exact bone sample.

## Recording and storage

Events Core stores the incident data used for historic replay under eventsCore/data/recordings by default. Measure available disk space and establish a backup policy before raising capture limits or retention periods. Recordings can contain player, location, and incident information.

## Optional integrations

CAD, Discord, dispatch, phone, inventory, EUP, voice metadata, and generated clips require separate configuration. Their shipped bridges are disabled, empty, or metadata-only until connected to compatible systems. Enable one integration at a time on a test server and keep credentials server-side.

Bodycam's standard historic replay works without an encoded video file. Any separate video-generation or upload workflow requires a compatible, trusted recorder or processing setup in Events Core.
