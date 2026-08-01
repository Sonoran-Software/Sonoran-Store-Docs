---
title: Changelog
published: true
date: 2026-08-01T00:00:00.000Z
tags: [changelog, release notes]
editor: markdown
dateCreated: 2026-07-28T00:00:00.000Z
description: Sonoran Station Displays release history, behavioral changes, and known limitations.
---

# Changelog

## 1.0.1 - August 1, 2026

### Fixed

* Fixed false persistence failures on FXServer builds that return stale file data after a filesystem fallback write. Successful display changes are now verified directly from disk so they remain saved after restart.
* Fixed mounted displays not appearing inside custom MLO interiors.

## 1.0.0 release candidate - July 28, 2026

### Added

* Persistent mounted displays with LEO, Fire/EMS, and combined filtering
* Active Units, Emergency Calls, Dispatch Calls, Live Map, and Bodycams pages
* Synchronized nearby page changes, temporary map panning, and rebindable controls
* Ordered page rotation, unit grouping/sorting, and list/bodycam pagination
* Bundled calibrated satellite map with unit, call, station, stale, bounds, and debug behavior
* Subscription-scoped local JPEG bodycam capture through `screenshot-basic`
* GTA/FiveM in-game clock and estimated weather header
* Self-contained display management, preview, placement, movement, and diagnostics
* ACE/custom permissions, server validation, event limits, and verified JSON persistence
* Optional queued administrative and security webhooks
* Client/server exports and documented DUI state protocol
* Distance-managed rendering with a configurable active-DUI limit

### Security and privacy

* Player mutations are revalidated server-side for permission, proximity, routing bucket, model, enum, range, string, page, and cooldown constraints.
* CAD/display state is sanitized and synchronized for nearby display rendering.
* Caller information is disabled by default and is not rendered by the current call card.
* Bodycam source ownership, display eligibility, MIME/base64 form, size, cadence, and
  recipients are validated server-side; arbitrary stream URLs are not accepted.
* Administrative webhook URLs remain server-only and mentions are disabled.

### Known limitations

* Active-unit rows do not display a dedicated stale badge.
* `VisibleCallFields.status`, independent postal visibility, and caller display are
  configured/normalized but not rendered by the current UI.
* `Config.MapRefreshInterval` is not consumed by the current renderer.
* `FOLLOW_UNITS` and `AUTO_FIT` can produce the same viewport for ordinary unit datasets.
* Runtime pause/resume exists internally but has no supported customer command or menu control.
* Named render targets and texture replacement cannot provide independent content for identical model instances.
* The bodycam bridge must be present in the installed SonoranCADFiveM build; `4.0.72` is the cache minimum, not a claim that every `4.0.72` package includes that bridge.

### Documentation

The July 28 final pass reconciled customer documentation against Station Displays,
SonoranCADFiveM, `radio_fivem`, shared store-resource conventions, and the live
renderer. Bodycam imagery now uses local periodic capture and no longer depends on the
previous TURN/remote-relay viewing path.
