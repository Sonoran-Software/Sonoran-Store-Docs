---
title: Diagnostics
published: true
date: 2026-07-28T00:00:00.000Z
tags: [diagnostics, commands, support]
editor: markdown
dateCreated: 2026-07-28T00:00:00.000Z
description: Use Station Displays in-game, server, permission, persistence, map, and bodycam diagnostics.
---

# Diagnostics

## In-game diagnostics

Open:

```text
/stationdisplay > Mounted Displays > View Diagnostics
```

The player needs `sonoran.stationdisplay.menu` to open the menu and
`sonoran.stationdisplay.diagnostics` to open diagnostics. The menu shows:

* CAD detected, compatible, version, and current error
* Configured and nearby display counts
* Active DUI count
* LEO and Fire/EMS unit counts
* Emergency and dispatch call counts
* Cache revision
* Bodycam runtime/capture dependency detected, available, and mode
* Source client, capture active, last attempt/success, and upload duration
* Latest frame size/sequence, subscribed viewers, and visible feed count
* Accepted, malformed, oversized, stale, failed, and rate-limited counters

Use **Refresh Diagnostics** for a new snapshot. The menu does not currently show webhook status, per-cache timestamps, map clipping, or each DUI's last update.

## Server diagnostic

Run in the server console:

```text
stationdisplay_serverdiag
```

It prints a JSON snapshot containing the CAD object, display and cache counts, cache revision, promotional-data status, and bodycam state. A player can run the command only with diagnostics permission. Running it creates a local administrative audit entry and, when configured, a webhook diagnostic entry.

## Permission diagnostic

When `Config.Debug = true`:

```text
stationdisplay_permissiondiag [serverId]
```

The console can inspect any source. A player needs diagnostics permission. Output covers logical action, backend action, ACE node, current menu snapshot, server action, and persistence diagnostics.

## Persistence repair

Console only:

```text
stationdisplay_repair_persistence
```

Use this only after the active persistence file is unreadable. The command backs it up as `data/displays.invalid.<epoch>.json`, resets from the shipped default, and requires a resource restart. See [Persistence](persistence.md).

## Map diagnostics

`Config.Map.debug = true` adds a visual projection overlay to Live Map. It reports map bounds, asset crop, clipping, and selected viewport data. This is separate from the in-game diagnostics menu.

## Refresh options

* **Force-Refresh Display** requires edit permission and refreshes one selected display.
* **Full Display Resync** requires diagnostics permission and resends the server snapshot.
* Restart/rejoin performs the normal initial synchronization.

None of these forces SonoranCADFiveM to rebuild its own caches.

Continue with [Troubleshooting](troubleshooting.md) for symptom-based checks and error codes.
