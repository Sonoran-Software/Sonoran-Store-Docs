---
title: Events Dashcam
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events dashcam, dashcam, evidence]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Configure and operate the Events moving-vehicle evidence and replay product.
---

# Events Dashcam

`eventsDashcam` is the vehicle evidence DLC over `eventsCore`. It owns vehicle
eligibility, server-observed vehicle binding, trigger intent, commands, key
mappings, product metadata, and evidence workflow requests.

The core owns the moving camera's rolling buffer, session, immutable snapshot,
structured replay, storage, retention, processing, CAD, Discord, and audit state.
Dashcam does not run a client sampler, chunk uploader, recording-file database,
native voice capture, or legacy media-URL replay.

## Start order

```cfg
ensure eventsCore
ensure eventsDashcam
```

The DLC fails closed when the core is missing, outdated, or unable to verify the
vehicle binding.

## Operator workflow

* `F7` opens the evidence panel.
* `F9` starts/stops a manual recording.
* `F10` adds a marker.
* `dashcam`, `dashcamrecord`, `dashcamsave`, `dashcammark`, `dashcamreview`, and
  `dashcamadmin` remain available as command aliases.

Manual sessions can last up to 1,800 seconds. Automatic lights, siren, collision,
speed, gunshot, dispatch, and integration triggers use the same session and add
validated core markers rather than creating parallel capture engines.

Continue with [Installation](installation.md), [Vehicle setup](vehicle-setup.md),
[Recording](recording.md), and [Playback](playback.md).
