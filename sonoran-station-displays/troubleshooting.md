---
title: Troubleshooting
published: true
date: 2026-07-28T00:00:00.000Z
tags: [troubleshooting, diagnostics, error codes]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Diagnose startup, CAD, blank displays, permissions, persistence, pages, map, bodycams, models, and performance.
---

# Troubleshooting

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

**Corrective action:** Install or update SonoranCADFiveM to version `4.0.72` or newer,
keep its resource name `sonorancad`, and start it before Station Displays.

**Relevant logs:**

| Code | Meaning |
| --- | --- |
| `SSD-CAD-001` | Configured CAD resource is not started |
| `SSD-CAD-002` | Required cache exports are unavailable |
| `SSD-CAD-003` | Version is below the supported minimum |

## Version checker does not run

**Likely cause:** The resource is a source or development build rather than an official
packaged release.

**Verify:** Confirm the resource was downloaded from the official granted asset.

**Corrective action:** Install the official packaged release. Development builds
intentionally retain the packaging token and skip outbound version requests.

## Update check fails

**Likely cause:** FXServer cannot reach the packaged version endpoint, the request timed
out, or the endpoint returned malformed metadata.

**Verify:** Review `SSD-UPDATE-001` or `SSD-UPDATE-002` in the server console and confirm
outbound HTTPS access.

**Corrective action:** Restore outbound HTTP access and retry at the next scheduled
check or resource start. An update-check failure does not stop the display resource.

## SonoranCAD version is unsupported

**Likely cause:** The installed `sonorancad` version is older than `4.0.72`.

**Corrective action:** Update SonoranCADFiveM to version `4.0.72` or newer so the public
unit, call, and emergency cache interfaces are available.

## Customer config contains removed settings

**Likely cause:** An older `config.lua` was copied over the current package.

**Corrective action:** Delete obsolete update-check, dependency, DUI
resolution/lifecycle, and event-rate-limit entries, then merge only documented
customer-facing settings into the current config. Do not modify protected
implementation files; internal values always take precedence.

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

**Relevant logs:** Temporarily enable `Config.Map.debug` to inspect calibrated bounds,
asset crop, clipping, and selected viewport values. Disable it afterward.

## Live Map markers are misaligned

**Likely cause:** A coordinate source uses another map convention, the record is outside
the calibrated GTA V bounds, or custom browser/map files were mixed between versions.

**Verify:** Restore the shipped `gtav-satellite-z4.webp` and `map-config.js`, enable the
map debug overlay, and compare a known landmark.

**Corrective action:** Replace the complete `web/` directory from one release and fix the
upstream world coordinates. Do not tune bounds to compensate for one malformed record.

## Bodycams do not appear

**Likely cause:** The installed CAD package lacks the bodycam state bridge,
`screenshot-basic` is not started, the bodycam is disabled/off duty, or the display
filter/page excludes it.

**Verify:** Open [Diagnostics](diagnostics.md) and check bodycam detected, available,
mode, active sources, capture dependency, and subscription counts.

**Corrective action:** Install a compatible SonoranCADFiveM bodycam package, start
`screenshot-basic`, put the unit on duty, activate the bodycam, and enable the
`BODYCAMS` page.

## Bodycam stays connecting or unavailable

**Likely cause:** The officer is loading/paused/faded, local capture failed, the JPEG
exceeded 300 KB, or the viewer subscription is out of range/ineligible.

**Verify:** Review last attempt/success, upload duration, frame size, failure reason,
source client, subscriber count, and malformed/oversized counters.

**Corrective action:** Restore `screenshot-basic`, let the officer finish loading,
reduce game resolution if frames exceed the limit, and verify range, routing bucket,
interior, current bodycam page, and service filter.

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

**Likely cause:** Missing `sonoran.stationdisplay.menu`, changed command name, placement already active, or a client error.

**Verify:**

* Check `Config.AdminCommand`.
* Test the configured command.
* Check ACE inheritance with the server's ACE tools/config.
* Check client F8.

**Corrective action:** Grant `sonoran.stationdisplay.menu`, finish/cancel placement, or
fix the reported client error.

**Relevant logs:** Permission-denied notification and F8 errors. No radio resource is required.

## Menu opens but an option reports no permission

**Likely cause:** The server-provided menu snapshot is older than the player's current
ACE state, the operation uses a different node, or a server recheck correctly rejected
the action.

**Verify:** Compare the requested action with [Permissions](permissions.md), then run
`stationdisplay_permissiondiag <serverId>` from the console while `Config.Debug = true`.

**Corrective action:** Grant the exact node, review inherited deny rules, and reconnect
or refresh the menu snapshot. Do not treat a visible item as authorization.

**Relevant permission:** Menu, place, edit, refresh, delete, diagnostics, and webhook
test are independent server checks.

## Cannot place or edit a display

**Likely cause:** Missing `place`/`edit`, final position farther than 15 meters, invalid value, routing-bucket mismatch, or event rate limit.

**Verify:**

* Confirm the exact ACE.
* Stay near the position.
* Keep numeric values in documented ranges.
* Review the on-screen rejection notification.

**Corrective action:** Grant the needed ACE, move closer, restore valid settings, or wait for the 10-second rate window.

**Relevant logs:** Client notification such as permission denied, invalid request, position, or bucket mismatch.

## Display saves but does not update live

**Likely cause:** Persistence succeeded but the client broadcast failed, the viewer lost
connectivity or world scope, or its DUI is unavailable.

**Verify:** Look for `CLIENT_BROADCAST_FAILED`, compare the persistence audit ID, run
`stationdisplay_serverdiag`, and inspect Active DUIs/client F8.

**Corrective action:** Restore client connectivity and world scope, use **Force-Refresh
Display**, or run **Full Display Resync** with diagnostics permission.
Do not replace a valid persistence file for a broadcast-only failure.

## Default JSON is not initialized

**Likely cause:** The shipped default is missing/invalid, the resource cannot write its
data directory, or an invalid active file already exists and was intentionally
preserved.

**Verify:** Check `data/displays.DEFAULT.json`, `data/displays.json`, and
`SSD-PERSIST-INIT-*`; `SSD-PERSIST-INIT-005` means the existing active file was not
overwritten.

**Corrective action:** Stop the resource, preserve the active file, restore a valid
default from the package, fix filesystem access, then restart. Use the console repair
command only when recovery from the active file is not possible.

## DUI readiness times out

**Likely cause:** A missing/mixed `web/` directory, browser JavaScript failure, or a
client DUI/runtime-texture problem prevented `duiReady` within 10 seconds.

**Verify:** Inspect client F8 and confirm all production web files come from the same
resource release.

**Corrective action:** Restore the complete `web/` directory, leave/re-enter range, and
let the internal one-second retry run. Repeated timeouts require client/runtime
investigation rather than increasing an internal timeout.

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

**Likely cause:** One page enabled, rotation disabled, invalid interval reset to default,
or a recent viewer interaction is inside the default 10-second hold.

**Verify:**

* Confirm two or more enabled pages.
* Confirm **Page Rotation** is enabled.
* Confirm interval is 5–300 seconds.
* Watch the footer dots/progress bar.

**Corrective action:** Enable multiple pages, save rotation, use a valid interval, wait
for the interaction hold, and force refresh if needed. A recreated DUI restores the
server runtime snapshot rather than always starting from the saved default.

**Relevant logs:** No dedicated rotation log. Use the displayed footer and current page.

## Nearby page prompt does not appear

**Likely cause:** No applicable action, outside the display's saved interaction
distance, poor view angle, failed line of sight, wrong interior/bucket, unavailable DUI,
or disabled page interaction.

**Verify:** Stand within 2.5 meters by default, face the screen, and test a display with
multiple pages or Live Map/Bodycams controls.

**Corrective action:** Correct the display's interaction distance and world context,
enable `Config.PageInteraction`, and remove geometry blocking line of sight. A one-page
display intentionally hides the prompt only when its current page has
no map/bodycam action.

## The wrong nearby display changes page

**Likely cause:** Another display has the better combined distance/view-angle target
score even if it is not the closest object.

**Verify:** Face the intended screen directly and move closer while watching the prompt
name.

**Corrective action:** Separate overlapping displays, shorten their interaction
distances, or approach the intended surface from a clearer angle. The server still
checks proximity and world context before broadcasting the change.

## Rotation resumes too soon after manual interaction

**Likely cause:** `PageInteraction.pauseRotationAfterInteraction` is low/zero, the
configuration was not restarted, or an integration forced another runtime page.

**Verify:** Confirm the default 10-second value and compare the timing with server
`SetDisplayPage` calls.

**Corrective action:** Restore the desired hold value and restart the resource. The
manual page change itself is runtime-only and does not change the saved default.

## Live Map is clipped or letterboxed

**Likely cause:** Normal aspect-ratio containment, a custom/mixed map asset, or an
incorrect model surface ratio.

**Verify:** The shipped map intentionally uses the actual contained image rectangle;
side bands can appear on a 16:9 viewport. Enable map debug and confirm markers align
inside the displayed image.

**Corrective action:** Restore the shipped asset/config and calibrate the model surface
to 16:9. Do not project markers across the letterboxed bands.

## Bodycam feed is delayed or fails to load

**Likely cause:** Local capture or decode failed, the frame was oversized, the officer
paused/loaded/disconnected, or the subscription expired.

**Verify:** Watch `DELAYED` versus `CAPTURE FAILED`/`OFFLINE`, check client logs, and
inspect capture timing, frame size, subscriber, malformed, oversized, and stale
counters.

**Corrective action:** Restore local capture readiness, reduce feed count, and
re-enter render range. The last frame remains briefly and is removed after 30 seconds.

## Administrative webhook does not arrive

**Likely cause:** Disabled/malformed private URL, Discord 4xx, outbound HTTPS failure,
rate/test cooldown, or a queue retry in progress.

**Verify:** Run the in-menu webhook test with
`sonoran.stationdisplay.webhook.test`, retain its audit ID, and inspect the server response log.

**Corrective action:** Regenerate invalid credentials, restore outbound HTTPS, and allow
429/5xx retries. The administrative action can still succeed because delivery is
non-blocking.

## High client resource usage

**Likely cause:** Too many nearby DUIs, long render distances, raised DUI resolution, high row limits, or many map markers.

**Verify:** Open diagnostics and compare Nearby Displays, Active DUIs, and `MaximumActiveDuis`.

**Corrective action:** Reduce render distances, nearby display count, resolution, row limits, or call markers. Keep no more than eight active DUIs with default limits.

**Relevant data:** Client profiler/F8 plus menu DUI counts.

## Diagnostics

See [Diagnostics](diagnostics.md) for the in-game snapshot, server/permission commands,
persistence repair, map overlay, full resync, and bodycam counters.

## Error and warning codes

| Code | Meaning |
| --- | --- |
| `SSD-CAD-001` | Configured SonoranCADFiveM resource is not started |
| `SSD-CAD-002` | Required CAD cache exports are missing |
| `SSD-CAD-003` | CAD version is below the supported minimum |
| `SSD-PERSIST-001` | Persistence JSON is malformed |
| `SSD-PERSIST-002` | One saved display failed validation |
| `SSD-PERSIST-003` | Encode/write/readback/validation/exact-state persistence verification failed |
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
