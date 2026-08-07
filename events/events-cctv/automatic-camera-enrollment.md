---
title: Automatic camera enrollment
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events cctv, mlo, camera discovery]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Enroll compatible map and MLO CCTV props as logical Events cameras.
---

# Automatic camera enrollment

Map-camera enrollment is disabled by default. When enabled, it discovers compatible
props already streamed by a map, MLO, or interior resource and converts them into
normal logical CCTV cameras. It does not retain the original object handle or run a
second recording system.

## Enable discovery

Edit `eventsCctv/config/cameras.lua`:

```lua
Config.AutoEnrollMapCameras.enabled = true
Config.AutoEnrollMapCameras.scanOnResourceStart = true
Config.AutoEnrollMapCameras.scanOnAreaChange = true
Config.AutoEnrollMapCameras.scanCooldown = 10000
Config.AutoEnrollMapCameras.scanRadius = 250.0
Config.AutoEnrollMapCameras.maxAutoEnrolledCameras = 500
Config.AutoEnrollMapCameras.activateOnDiscovery = true
Config.AutoEnrollMapCameras.persistDiscoveredCameras = true
```

The shipped model list is:

```text
prop_cctv_cam_01a, prop_cctv_cam_01b, prop_cctv_cam_02a,
prop_cctv_cam_03a, prop_cctv_cam_04a, prop_cctv_cam_04b,
prop_cctv_cam_04c, prop_cctv_cam_05a, prop_cctv_cam_06a,
prop_cctv_cam_07a
```

Add a custom model to both `Config.AutoEnrollMapCameras.models` and
`Config.AllowedCameraModels`, then add its entry to
`Config.AutoEnrollCameraModelSettings`:

```lua
Config.AutoEnrollCameraModelSettings['my_mlo_cctv'] = {
    positionOffset = { x = 0.0, y = -0.35, z = 0.15 },
    rotationOffset = { x = -15.0, y = 0.0, z = 180.0 },
    fov = 65.0
}
```

## What the server validates

The client submits model and transform candidates in bounded batches. The server
checks the model allowlist, transform limits, model offset, stable ID, duplicates,
permission, request rate, and the maximum camera count before registering the
logical camera through the existing CCTV-to-core contract.

Automatic records use the `map_auto` group and the logical terminal ID
`map_auto_discovery`. A nearby configured terminal can supply organization metadata
without changing the permission boundary. Manual cameras take precedence over an
automatic duplicate.

## Commands

All commands use the existing `cctv.admin` or configured framework authorization:

```text
/camerascan
/cameraautoscanstatus
/cameracleanauto confirm
/cameracleanauto stale confirm
```

`/camerascan` performs an authorized local scan even when automatic scanning is
disabled. The status command reports counters, timing, stale records, and
persistence. Cleanup requires the literal `confirm`; it does not remove manual or
configured cameras.

## MLO guidance

MLO objects may stream only after a player enters the interior and may later stream
out. Use area-change scanning or `/camerascan` after entry. The stored logical
camera remains usable after the original object handle disappears because the
validated transform is retained.

Stale cleanup is disabled by default. If enabled, review repeated missing-scan
status before using the confirmed cleanup command.
