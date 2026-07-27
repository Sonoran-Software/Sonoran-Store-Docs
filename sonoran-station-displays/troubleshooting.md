---
title: Troubleshooting and Diagnostics
published: true
date: 2026-07-27T00:00:00.000Z
tags: [troubleshooting, diagnostics, error codes]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Diagnose startup, CAD cache, blank display, permissions, persistence, rotation, model, and performance problems.
---

# Troubleshooting and Diagnostics

## Resource does not start

**Likely cause:** Incorrect folder nesting, missing manifest files, Lua error, or an incomplete escrow download.

**Verify:**

* `fxmanifest.lua` is directly inside `resources/.../sonoran-stationdisplay/`.
* The server console identifies the first file/error.
* `web/index.html`, `web/style.css`, `web/app.js`, and `data/displays.DEFAULT.json` exist.

**Corrective action:** Re-extract the package, remove any duplicate outer folder, and restore the complete resource. Do not combine files from different versions.

**Relevant logs:** FXServer startup errors before the normal `[Sonoran Station Displays]` messages.

## SonoranCADFiveM is not detected

**Likely cause:** Wrong resource name, wrong start order, version below `4.0.72`, or missing cache exports.

**Verify:**

```cfg
ensure sonorancad
ensure sonoran-stationdisplay
```

Run from the server console:

```text
stationdisplay_serverdiag
```

**Corrective action:** Start/update SonoranCADFiveM and make `Config.SonoranCADResource` match its folder name.

**Relevant logs:**

| Code | Meaning |
| --- | --- |
| `SSD-CAD-001` | Configured CAD resource is not started |
| `SSD-CAD-002` | Required cache exports are unavailable |
| `SSD-CAD-003` | Version is below the configured minimum |

## Display is blank

**Likely cause:** Outside render distance, DUI pool full, invalid/unloaded model, incorrect custom surface geometry, missing web files, or a client script error.

**Verify:**

* Walk close to the display.
* Open diagnostics and check **Active DUIs**.
* Compare with `Config.MaximumActiveDuis`.
* Check the client F8 console.
* Confirm the model exists in `Config.DisplayModels`.

**Corrective action:** Use **Force-Refresh Display**, leave/re-enter range, reduce overlapping render distances, restore web files, or correct the model registry. For identical props, use `WORLD_PLANE`.

**Relevant logs:** Client F8 errors for DUI creation, NUI JavaScript, model loading, or render targets.

## Display shows no units

**Likely cause:** No matching on-duty cache entries, offline status, wrong service filter, or **Hide Unavailable Units**.

**Verify:**

```text
sonorancad viewcaches
```

Confirm the unit is in the active unit cache and inspect `data.page` and status.

**Corrective action:** Put the unit on duty in CAD, select the correct LEO/Fire/EMS/Both filter, or disable **Hide Unavailable Units**.

**Relevant diagnostics:** LEO Units, Fire/EMS Units, CAD Compatible, cache revision.

## LEO or Fire/EMS unit is in the wrong section

**Likely cause:** Incorrect `unit.data.page` in the CAD cache.

**Verify:**

* `0` should be LEO.
* `1` and `2` should be Fire/EMS.
* `3` and unknown values become `OTHER`.

**Corrective action:** Fix the upstream cache classification. Use advanced service overrides only for a confirmed custom/legacy cache.

**Relevant logs:** Debug output and `sonorancad viewcaches`; no department list is normally required.

## Calls do not appear

**Likely cause:** Wrong page/cache, closed dispatch call, service mismatch, unclassified call hidden on a single-service display, or unavailable call cache.

**Verify:**

* Emergency Calls reads `GetEmergencyCache()`.
* Dispatch Calls reads `GetCallCache()`.
* Check assignments and call status.
* Compare emergency/dispatch counts in diagnostics.

**Corrective action:** Select the correct page, keep the call active in the source cache, assign a classified unit, use a Both display, or intentionally enable the unclassified fallback.

**Relevant diagnostics:** Emergency Calls, Dispatch Calls, CAD error, cache revision.

## Live Map is empty

**Likely cause:** Matching units/calls have no usable X/Y coordinates, markers are filtered, or the fixed region excludes them.

**Verify:**

* Confirm list pages contain records.
* Inspect CAD coordinate fields.
* Check map mode, center, zoom, **Show Calls**, and the service filter.

**Corrective action:** Restore upstream coordinates, use Auto Fit, correct the fixed center/zoom, or enable the required markers.

**Relevant logs:** There is no per-marker projection log. Use cached records and the displayed count.

## Display is visible but frozen

**Likely cause:** CAD cache stopped changing, connection is unavailable, DUI state is not being resent, or the browser encountered an error.

**Verify:**

* Check `CAD LIVE` versus the disconnected overlay.
* Refresh diagnostics and watch cache revision.
* Run `stationdisplay_serverdiag`.
* Check client F8.

**Corrective action:** Restore SonoranCADFiveM, use **Force-Refresh Display**, then leave/re-enter range. Restart `sonoran-stationdisplay` only after preserving persistence if the client state remains unhealthy.

**Relevant data:** Cache revision and the server diagnostic CAD object. The menu does not show per-DUI last-update timestamps.

## Menu does not open

**Likely cause:** Missing `sonoran.display.view`, changed command name, placement already active, or a client error.

**Verify:**

* Check `Config.AdminCommand`.
* Test the configured command.
* Check ACE inheritance with the server's ACE tools/config.
* Check client F8.

**Corrective action:** Grant `view`, finish/cancel placement, or fix the reported client error.

**Relevant logs:** Permission-denied notification and F8 errors. No radio resource is required.

## Cannot place or edit a display

**Likely cause:** Missing `place`/`edit`, final position farther than 15 meters, invalid value, routing-bucket mismatch, or event rate limit.

**Verify:**

* Confirm the exact ACE.
* Stay near the position.
* Keep numeric values in documented ranges.
* Review the on-screen rejection notification.

**Corrective action:** Grant the needed ACE, move closer, restore valid settings, or wait for the 10-second rate window.

**Relevant logs:** Client notification such as permission denied, invalid request, position, or bucket mismatch.

## Display disappears at a distance

**Likely cause:** Normal distance-based cleanup.

**Verify:** Compare distance with the display's saved render distance. The object should disappear after the 3-second default destroy delay.

**Corrective action:** Increase the per-display render distance only as much as needed. Maximum accepted value is 250 meters.

**Relevant diagnostics:** Active DUI count should decrease after cleanup.

## Custom television stays black or shares content

**Likely cause:** Invalid named render target, global texture replacement, missing model, or incorrect world-plane offsets.

**Verify:**

* Switch the model entry to `WORLD_PLANE`.
* Test one instance and then two identical instances with different pages.
* Confirm model validity and local screen geometry.

**Corrective action:** Calibrate `screen.center`, `width`, `height`, and `flipX`. Do not rely on a render-target name alone.

**Relevant logs:** Client model/render errors; diagnostics only reports active DUI availability, not named-target validity.

## Settings reset after restart

**Likely cause:** `data/displays.json` cannot be loaded, one entry failed validation, or the resource folder was replaced without restoring runtime data.

**Verify:** Check the file and startup log's loaded display count.

**Corrective action:** Stop the resource, restore a valid backup, confirm write permission, and restart.

**Relevant logs:** `SSD-PERSIST-001`, `SSD-PERSIST-002`, or `SSD-PERSIST-003`.

## Page rotation is not working

**Likely cause:** One page enabled, rotation disabled, invalid interval reset to default, or a local page renderer was recreated.

**Verify:**

* Confirm two or more enabled pages.
* Confirm **Page Rotation** is enabled.
* Confirm interval is 5–300 seconds.
* Watch the footer dots/progress bar.

**Corrective action:** Enable multiple pages, save rotation, use a valid interval, and force refresh.

**Relevant logs:** No dedicated rotation log. Use the displayed footer and current page.

## High client resource usage

**Likely cause:** Too many nearby DUIs, long render distances, raised DUI resolution, high row limits, or many map markers.

**Verify:** Open diagnostics and compare Nearby Displays, Active DUIs, and `MaximumActiveDuis`.

**Corrective action:** Reduce render distances, nearby display count, resolution, row limits, or call markers. Keep no more than eight active DUIs with default limits.

**Relevant data:** Client profiler/F8 plus menu DUI counts.

## Diagnostics

### In-game diagnostics

Open:

```text
/stationdisplay → Mounted Displays → View Diagnostics
```

Requires `sonoran.display.diagnostics`.

The menu shows:

* CAD detected, compatible, version, and error
* Configured display count
* Nearby display count
* Active DUI count
* LEO and Fire/EMS unit counts
* Emergency and dispatch call counts
* Cache revision

Use **Refresh Diagnostics** to request a new snapshot.

### Server diagnostics

From the server console:

```text
stationdisplay_serverdiag
```

The console may always run it. A player running it requires diagnostics permission.

Output includes:

* CAD availability/compatibility/version/error object
* Configured display count
* Total normalized unit count
* Emergency and dispatch call counts
* Cache revision

### Available refresh actions

* **Force-Refresh Display** — selected display, requires edit
* Server `RefreshDisplay` export — selected display runtime resend
* Rejoin/resource start — internal full synchronization request

Version 1.0.0 has no public full-resync command, coordinate-projection status command, or CAD-cache force-rebuild action. The menu does not expose current page, last cache update, last DUI update, or render-target status as individual fields.

## Error and warning codes

| Code | Meaning |
| --- | --- |
| `SSD-CAD-001` | Configured SonoranCADFiveM resource is not started |
| `SSD-CAD-002` | Required CAD cache exports are missing |
| `SSD-CAD-003` | CAD version is below the supported minimum |
| `SSD-PERSIST-001` | Persistence JSON is malformed |
| `SSD-PERSIST-002` | One saved display failed validation |
| `SSD-PERSIST-003` | Display data could not be encoded |
| `SSD-UPDATE-001` | Remote version request failed |
| `SSD-UPDATE-002` | Remote version payload is invalid |
| `SSD-UPDATE-003` | A newer resource version is available |

## Support checklist

Before contacting [Sonoran Support](https://support.sonoransoftware.com/), collect:

* Sonoran Station Displays version
* SonoranCADFiveM version
* FXServer artifact version
* Relevant `config.lua` sections with secrets and identifiers removed
* Server console errors and `stationdisplay_serverdiag` output
* Client F8 errors
* In-game diagnostic values
* Display ID/name
* Display model and configured render method/target
* Current service filter and page
* Exact reproduction steps

Never post CAD API keys, Tebex credentials, license credentials, private caller data, or unredacted player identifiers.
