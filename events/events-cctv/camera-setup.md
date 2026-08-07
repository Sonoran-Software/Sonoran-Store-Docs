---
title: CCTV camera setup
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events cctv, cameras, mlo]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Configure CCTV terminals, camera defaults, presets, model allowlists, and custom MLO camera transforms.
---

# CCTV camera setup

## Shipped camera defaults

The default camera prop is `prop_cctv_cam_01a`. Cameras use a 60 degree field of
view, 25-90 degree zoom bounds, a 75 m capture radius, zoom enabled, pan and tilt
disabled, a 10-second per-entity motion cooldown, and recording/motion enabled.
The maximum is 32 cameras per terminal and a 250 m capture radius.

## Terminals and persistence

The default terminal prop is `prop_monitor_01c`. Terminal state and camera records
are persisted to `data/cctv.json` when `Config.Persistence.Enabled` is true. Product
presets such as `convenience_stores` and `law_enforcement` are opt-in and do not
spawn props by default.

## Custom camera models

Map/MLO models must be added to both the client discovery list and the server
allowlist before automatic enrollment can accept them. A custom model also needs a
model-specific position offset, rotation offset, and field of view. The offsets are
calibrated to the model's local axes; CCTV meshes do not all point along the same
forward direction.

Prefer a manual camera first. Confirm its live view, replay perspective, interior
context, routing bucket, and duplicate behavior before enabling automatic discovery.

See [Automatic camera enrollment](automatic-camera-enrollment.md) for the complete
allowlist and MLO workflow.
