---
title: CCTV troubleshooting
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events cctv, troubleshooting]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Diagnose CCTV startup, camera, live-view, enrollment, persistence, and replay issues.
---

# CCTV troubleshooting

## `/cctv` does nothing

Confirm `eventsCctv` is running after `eventsCore`, then check the player's
`cctv.admin` or product permission. Resource-name changes and a nested folder can
prevent the manifest dependency from resolving.

## A camera is visible but cannot be viewed

Check `cctv.live.view`, terminal group/specific access, routing bucket, camera
enabled state, and the live-view viewer cap. A camera can be present in product
state while still being hidden from a viewer who lacks the relevant permission.

## Map cameras are not found

Enrollment is disabled by default. If it is enabled, enter the MLO so objects are
streamed, use `/camerascan`, verify both allowlists, and calibrate the model offset.
Use `/cameraautoscanstatus` to inspect counters and persistence mode.

## A map camera disappeared after restart

Confirm both CCTV persistence and `persistDiscoveredCameras` are enabled. If the
latter is false, automatic records are intentionally memory-only. Review stale
cleanup settings before removing any record.

## Replay is empty or has no encoded file

Confirm the session finalized, the viewer has `cctv.recording.view`, and a recorder
worker is registered if an MP4/WebM artifact is expected. Structured replay does
not require an encoder.

For broader dependency, storage, and live-environment checks, see [Events
Troubleshooting](../troubleshooting.md).
