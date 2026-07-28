---
title: Changelog
published: true
date: 2026-07-28T00:00:00.000Z
tags: [changelog, release notes]
editor: markdown
dateCreated: 2026-07-28T00:00:00.000Z
description: Sonoran Station Displays release history, behavioral changes, and known limitations.
---

# Changelog

## 1.0.0 release candidate - July 28, 2026

### Added

* Persistent mounted displays with LEO, Fire/EMS, and combined filtering
* Active Units, Emergency Calls, Dispatch Calls, Live Map, and Bodycams pages
* Synchronized nearby page changes, temporary map panning, and rebindable controls
* Ordered page rotation, unit grouping/sorting, and list/bodycam pagination
* Bundled calibrated satellite map with unit, call, station, stale, bounds, and debug behavior
* Passive PeerJS/WebRTC bodycam viewing through a compatible SonoranCADFiveM runtime
* GTA/FiveM in-game clock and estimated weather header
* Self-contained display management, preview, placement, movement, and diagnostics
* ACE/custom permissions, server validation, event limits, and verified JSON persistence
* Optional queued administrative and security webhooks
* Client/server exports and documented DUI state protocol
* Distance-managed rendering with a configurable active-DUI limit

### Security and privacy

* Player mutations are revalidated server-side for permission, proximity, routing bucket, model, enum, range, string, page, and cooldown constraints.
* CAD/display state is sent only to authorized viewers.
* Caller information is disabled by default and is not rendered by the current call card.
* Bodycam viewer hosts are pinned to Sonoran's PeerJS service; arbitrary client stream URLs are rejected.
* Administrative webhook URLs remain server-only and mentions are disabled.

### Known limitations

* Active-unit rows do not display a dedicated stale badge.
* `VisibleCallFields.status`, independent postal visibility, caller display, and bodycam `showTimestamp` are configured/normalized but not rendered by the current UI.
* `Config.MapRefreshInterval` is not consumed by the current renderer.
* `FOLLOW_UNITS` and `AUTO_FIT` can produce the same viewport for ordinary unit datasets.
* Runtime pause/resume exists internally but has no supported customer command or menu control.
* Named render targets and texture replacement cannot provide independent content for identical model instances.
* The bodycam bridge must be present in the installed SonoranCADFiveM build; `4.0.72` is the cache minimum, not a claim that every `4.0.72` package includes that bridge.

### Documentation

The July 28 final pass reconciled customer documentation against Station Displays, SonoranCADFiveM, `radio_fivem`, shared store-resource conventions, and the live renderer. It replaces earlier descriptions of a coordinate grid, local-only page interaction, 3-meter targeting, four pages, or screenshot-based bodycams.
