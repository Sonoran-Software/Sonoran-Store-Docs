---
title: CCTV installation
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events cctv, installation]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Install Events CCTV, configure the core dependency, and perform a first camera test.
---

# CCTV installation

Install `eventsCore` first using the suite [Installation](../installation.md)
guide. Then place `eventsCctv` as a sibling resource with `fxmanifest.lua` at its
root.

## Start order

```cfg
set onesync on
ensure eventsCore
ensure eventsCctv
```

The CCTV DLC blocks provider registration if `eventsCore` is missing or below the
minimum compatible version.

## First configuration

Review these files before inviting operators:

* `eventsCctv/config/config.lua`: interaction, target system, live view, remote
  view, motion, watermark, and persistence.
* `eventsCctv/config/cameras.lua`: camera defaults, model allowlists, presets, and
  optional map enrollment.
* `eventsCctv/config/permissions.lua`: ACE nodes and terminal access prefixes.
* `eventsCctv/config/remote.lua`: the server-only trusted bridge allowlist.

Leave map enrollment disabled until the base terminal/camera workflow is tested.

## First test

1. Grant `cctv.admin`, `cctv.live.view`, `cctv.recording.view`, and
   `cctv.recording.create` to a test group.
2. Open `/cctv` and create a terminal in a test location.
3. Add a camera, verify live view, and trigger a motion/bookmark event.
4. Wait for post-roll, open the archive, and request structured replay.
5. Move or remove the live camera and confirm the finalized session retains its
   captured camera perspective.

The test proves the product flow only in the target server when OneSync and GTA
natives are available; repository tests do not replace that smoke test.
