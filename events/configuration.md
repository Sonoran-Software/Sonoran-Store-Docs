---
title: Configuration
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, configuration, events]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Configure capture limits, storage, permissions, integrations, CCTV behavior, and Dashcam policy.
---

# Configuration

All shipped configuration values are conservative defaults. Validate changes on a
test server before applying them to an evidence archive.

## Core configuration

`eventsCore/config/config.lua` contains shared limits:

| Setting | Shipped value |
| --- | ---: |
| `SchemaVersion` | `1` |
| `Framework` | `standalone` |
| `MaxEventDuration` | `1800` seconds |
| `MaxEntities` | `64` |
| `MaxParticipants` | `32` |
| `SnapshotInterval` | `250` ms |
| `PreEventBuffer` / `PostEventBuffer` | `15` seconds each |
| Reconstruction speed | `1.0x`, minimum `0.25x`, maximum `8.0x` |
| Processing workers | `1` |
| Processing queue | `100` jobs, `3` attempts, `10` second retry delay, `600` second timeout |
| Retention | `7` days by default, `90` days maximum |
| Audit | enabled, maximum `1000` entries |

Set `Config.Framework` to `esx`, `qbcore`, `qbox`, or `auto` only when that
deployment's server context is available. The default permission source remains
ACE.

## Server-only files

`eventsCore/config/storage.lua` sets local artifact root to
`data/recordings`, maximum local storage to `10737418240` bytes (10 GiB), and
maximum file size to `536870912` bytes (512 MiB). Allowed MIME types are MP4,
WebM, JSON, JPEG, and PNG. External playback hosts must be listed explicitly in
`Config.Storage.External.PlaybackAllowedHosts`.

`eventsCore/config/integrations.lua` disables CAD and Discord by default. Each
integration has a `15000` ms request timeout and `3` attempts. CAD uses an endpoint
and API token; Discord uses a global webhook and the shipped username `Events
System`. Keep those values server-only.

`eventsCore/config/permissions.lua` enables deny-overrides, uses
`events.processing.worker` for worker authorization, disables automatic player
workers, and starts with an empty trusted worker-resource allowlist. Add a recorder
or uploader resource explicitly before it can register.

## Version checks

Each resource has an editable `config/update.lua` containing only
`Config.Updater.EnableAutoUpdate`. It is `false` by default. The Store endpoint,
startup delay, six-hour interval, stable channel, timeout, logging behavior, and
core/DLC compatibility minimums are protected implementation values and cannot be
changed by customer configuration. The checker reports version information and
never replaces files at runtime. See [Updating](updating.md).

## CCTV configuration

The shipped CCTV defaults are:

* text interaction, `prop_monitor_01c` terminal prop, 2 m interaction distance,
  15 m draw distance, and 500 ms state refresh;
* live-view transition `350` ms, maximum `8` viewers per camera, and disabled
  player controls while viewing;
* remote view enabled but requiring terminal remote access, with 300-second
  session TTL;
* motion polling every `500` ms, 10-second cooldown, and 0.75 m minimum movement;
* watermark `Sonoran Security`, timestamps, and night vision enabled; thermal view
  disabled;
* persistence enabled at `data/cctv.json`.

Map-camera enrollment is separate and disabled by default. Follow [Automatic
camera enrollment](events-cctv/automatic-camera-enrollment.md) before enabling it.

## Dashcam configuration

The shipped Dashcam capture policy uses a 60-second pre-roll, 20-second automatic
post-roll, 1,800-second maximum duration, and six maximum replay views. The
recording panel is `F7`, record is `F9`, and marker is `F10`. Manual, emergency
lights, siren, gunshot, collision, and speed triggers are enabled by default; CAD
and Discord product toggles are disabled by default.

Vehicle eligibility defaults to model `police`, emergency vehicle class support,
police/sheriff/ambulance/fire jobs, and inventory item `dashcam`. Review
[Vehicle setup](events-dashcam/vehicle-setup.md) before changing eligibility.
