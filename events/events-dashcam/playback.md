---
title: Dashcam playback
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events dashcam, replay, evidence]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Review structured Dashcam sessions with six validated camera views and evidence controls.
---

# Dashcam playback

Dashcam replay is structured reconstruction from the core-owned timeline. It is
not a client screenshot stream, native video, or media URL replay.

## Six views

| ID | Label | Field of view |
| --- | --- | ---: |
| `front` | FRONT | 72 degrees |
| `rear` | REAR | 70 degrees |
| `cabin` | CABIN | 85 degrees |
| `prisoner` | PRISONER | 90 degrees |
| `left` | LEFT | 68 degrees |
| `right` | RIGHT | 68 degrees |

The views are immutable definitions on the moving vehicle camera. A finalized
session retains its camera snapshot even when the vehicle or live camera is gone.

## Controls

The interactive replay supports pause, play, restart, seek, skip, step, speed,
view, and exit. The shipped core speed range is 0.25x to 8x; the Dashcam product
UI exposes the configured replay controls and validates every action server-side.

On exit, disconnect, timeout, or resource stop, the reconstruction worker restores
the viewer's visibility, freeze, and routing state.

## Evidence actions

Authorized operators can list and review evidence, add notes and markers, attach
incident metadata, classify, lock, preserve, archive, change retention, and delete
with a substantive reason. Every operation is reauthorized server-side and remains
core-owned.
