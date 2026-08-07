---
title: CCTV live view and playback
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events cctv, playback, live view]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Use CCTV live view, motion recordings, structured replay, remote sessions, and evidence actions.
---

# CCTV live view and playback

## Live view

Live view is a permission- and routing-aware scripted camera. The shipped transition
is 350 ms and each camera allows up to eight viewers. Player controls are disabled
while viewing. Zoom is enabled by default; pan and tilt depend on each camera's
capabilities.

Live view does not itself finalize a recording. Motion and manual bookmark workflows
request a core session when the configured recording permissions and camera state
allow it.

## Motion and bookmarks

Motion polling runs every 500 ms by default with a 0.75 m movement threshold and a
10-second cooldown. The server verifies that the entity was visible to the camera
in the correct routing scope before accepting the event. The client report is not
the authority.

## Archive and replay

The archive lists only sessions permitted by the core and product policy. Requesting
replay creates a structured reconstruction with the event-time camera definition
and transform snapshot. Moving or deleting a live camera does not rewrite a
finalized session's historical perspective.

An encoded video artifact requires a trusted recorder worker registered with the
core. See [Core storage and retention](../events-core/storage-and-retention.md).

## Remote view

Remote view is enabled in the shipped configuration but requires terminal remote
access. The bridge is server-only, must be allowlisted in `config/remote.lua`, and
uses caller-bound sessions with a 300-second TTL. There is no public HTTP listener
or browser credential in the resource.

## Display overlays

The shipped CCTV presentation enables the `Sonoran Security` watermark, timestamps,
and night vision. Thermal mode is disabled by default. Change these values only in
the supported configuration and review privacy expectations on the target server.
