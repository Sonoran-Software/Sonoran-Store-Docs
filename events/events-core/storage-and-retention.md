---
title: Core storage and retention
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events core, storage, retention]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Core-owned recording storage, artifact validation, retention, and evidence holds.
---

# Core storage and retention

The core is the only suite resource that owns finalized session storage and
retention. Product DLCs request core operations; they do not create parallel
recording databases or cleanup workers.

## Defaults

* Metadata and local artifacts: `eventsCore/data/recordings`.
* Maximum local total: 10 GiB (`10737418240` bytes).
* Maximum local file: 512 MiB (`536870912` bytes).
* Allowed MIME types: MP4, WebM, JSON, JPEG, and PNG.
* Default retention: 7 days.
* Maximum configured retention: 90 days.
* Audit: enabled, maximum 1,000 entries.

External playback is restricted to the exact HTTPS hosts listed in
`Config.Storage.External.PlaybackAllowedHosts`.

## Evidence state

Sessions carry event markers, notes, provider metadata, classification, retention,
archive, lock, and preserve state. Delete operations are authorized, audited, and
require a substantive reason where the product contract requires one. A preserved
session is not removed by normal retention cleanup.

## Restart behavior

Finalized metadata is reloaded from the core-owned storage index on restart. A
provider camera can be deregistered without rewriting the historical camera
snapshot in a finalized session.
