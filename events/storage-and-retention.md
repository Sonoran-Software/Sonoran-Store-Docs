---
title: Storage and retention
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, storage, retention, evidence]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Understand Events metadata, local artifacts, retention classifications, and optional media workers.
---

# Storage and retention

## What is stored

`eventsCore` owns finalized session metadata, structured timelines, immutable camera
snapshots, event markers, processing state, retention state, and audit records.
Local artifacts use the server-only root `eventsCore/data/recordings` by default.
`eventsCctv` persists product camera and terminal state in
`eventsCctv/data/cctv.json`.

The product does not automatically import recording files from an older standalone
recorder or Dashcam system.

## Local limits

The shipped core limits are:

* local storage total: `10737418240` bytes (10 GiB);
* one local file: `536870912` bytes (512 MiB);
* allowed MIME types: `video/mp4`, `video/webm`, `application/json`, `image/jpeg`,
  and `image/png`.

Change limits only after measuring the target server's disk capacity and backup
policy. A rejected artifact does not make the structured session a video file.

## Retention

The core default is 7 days and the shipped maximum is 90 days. The Dashcam product
classifications map to these defaults:

| Classification | Days |
| --- | ---: |
| `routine` | 7 |
| `traffic_stop` | 30 |
| `arrest` | 90 |
| `use_of_force` | 365 |
| `training` | 30 |
| `indefinite` | 0 (no expiry) |

Lock/preserve state and archive state are evidence controls; they should be part
of the server's documented evidence policy. Deleting evidence requires an
authorized request and a substantive reason.

## Encoded video and external playback

Structured replay works without an encoder. To generate MP4/WebM, register a
trusted recorder worker and add its resource to the server-only worker allowlist.
The worker returns an artifact to the core; it does not own the recording session
or retention policy.

External playback URLs must use an HTTPS host explicitly listed in
`Config.Storage.External.PlaybackAllowedHosts`. Do not allow arbitrary hosts.

## Backups and privacy

Back up the `data/` files with the same care as CAD records. They can contain player,
vehicle, incident, and evidence metadata. Do not put API tokens or Discord webhooks
inside persisted JSON files or client-accessible configuration.
