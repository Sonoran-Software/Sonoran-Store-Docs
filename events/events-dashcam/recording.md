---
title: Dashcam recording
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events dashcam, recording, triggers]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Configure Dashcam capture windows, automatic triggers, markers, and evidence classifications.
---

# Dashcam recording

## Capture defaults

The moving camera uses an 85 m capture radius, 60-second pre-roll, 20-second
automatic post-roll, 1,800-second maximum duration, and six maximum replay views.
Each eligible vehicle has one rolling buffer and one active core session.

## Triggers

The shipped trigger configuration polls every 500 ms and enables manual, emergency
lights, siren, gunshot, collision, and speed triggers. Gunshot radius is 65 m,
collision threshold is a 35-point damage delta, speed threshold is 44.7 m/s, and
speed-trigger cooldown is 30 seconds.

Automatic triggers add validated markers to the active session. They do not create
six separate sessions. Dispatch and integration hooks also send trigger intent and
metadata through the core contract.

## Keys and commands

| Action | Default key | Command |
| --- | --- | --- |
| Open panel | `F7` | `/dashcam` |
| Start/stop | `F9` | `/dashcamrecord` |
| Add marker | `F10` | `/dashcammark [label]` |
| Save buffer | - | `/dashcamsave` |
| Review | - | `/dashcamreview` |

The `dashcamadmin` alias opens the product admin surface. Debug commands are
intended for bounded diagnostics and should not be treated as a production capture
workflow.

## Evidence classification

The shipped retention mapping is `routine` 7 days, `traffic_stop` 30 days,
`arrest` 90 days, `use_of_force` 365 days, `training` 30 days, and `indefinite`
0 (no expiry). Review [Storage and retention](../storage-and-retention.md) before
changing the policy.
