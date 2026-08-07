---
title: Events CCTV
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events cctv, cctv, cameras]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Record incidents and reconstruct historical CCTV footage for later in-game review.
---

# Events CCTV

`eventsCctv` is a separately entitled paid DLC that requires the free `eventsCore`
package. Its primary workflow is historical evidence: motion detections and operator
bookmarks capture activity with pre-event and post-event context so authorized staff
can reconstruct and replay an incident after the people, vehicles, and action have
left the scene.

During replay, operators can pause, seek, skip backward or forward, step through
the timeline, restart, and change playback speed. CCTV also owns terminals, logical
camera definitions, placement, live PTZ and zoom viewing, motion/bookmark intent,
permissions, persistence, and the operator interface.

It does not own recording buffers, finalized timelines, reconstruction, video
files, uploads, retention cleanup, CAD, or Discord queues. Those remain core-owned.

## Start order

```cfg
ensure eventsCore
ensure eventsCctv
```

Open the operator interface with:

```text
/cctv
```

## Main features

* Historical incident capture with pre-event and post-event context.
* In-game reconstruction with pause, seek, skip, step, restart, and speed controls.
* Immutable captured camera angles that remain valid after a live camera moves or
  is deleted.
* Persistent terminals and cameras with server-authoritative CRUD.
* Coordinate and in-world placement with distance, routing, and short-lived grant
  validation.
* Live scripted-camera view with viewer caps, zoom, and cleanup.
* Motion detection and manual bookmark events validated against core visibility.
* Recording archive actions through core-owned sessions.
* Text, `ox_target`, `qb-target`, and automatic target-detection modes with a safe
  text fallback.
* Optional, opt-in enrollment of compatible CCTV props already streamed by a map,
  MLO, or interior resource.

## Product data

Configured terminal and camera state persists to `eventsCctv/data/cctv.json` when
persistence is enabled. Recording metadata and artifacts remain under
`eventsCore/data/recordings`. See [Camera setup](camera-setup.md), [Playback](playback.md),
and [Troubleshooting](troubleshooting.md).
