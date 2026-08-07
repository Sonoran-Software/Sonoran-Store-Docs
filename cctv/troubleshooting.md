---
title: Troubleshooting
published: true
date: 2026-08-07T00:00:00.000Z
tags: [fivem, cctv, troubleshooting]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Resolve common CCTV installation, camera, permission, recording, playback, and persistence problems.
---

# CCTV troubleshooting

Begin with the FXServer console and confirm both resources are current. Enable debug output only temporarily on a test server.

## CCTV will not start

Confirm the exact folder names and start order:

~~~cfg
ensure eventsCore
ensure eventsCctv
~~~

Each fxmanifest.lua must be directly inside its resource folder. CCTV requires the free Events Core package and will not initialize correctly if Core is missing, starts later, or is below the required version.

## /cctv does nothing

Confirm eventsCctv is running and the player has an appropriate permission such as cctv.admin, cctv.live.view, or cctv.recording.view. Check the server console for denied permission or configuration messages.

## The interface opens but an action is denied

Check:

* The ACE node for the requested action
* The player's ACE group membership
* Terminal-specific access with <code>cctv.terminal.&lt;terminalId&gt;</code>
* Remote group access with <code>cctv.remote.&lt;group&gt;</code>
* The resource's server-only permission configuration

## A camera is listed but cannot be viewed

Verify cctv.live.view, terminal access, the camera's enabled state, routing bucket, distance rules, and the live-view viewer limit. Reopen the interface after correcting access.

## A recording does not appear

Confirm:

1. Recording is enabled on the camera.
2. Motion triggers are enabled or an authorized bookmark was created.
3. The subject was inside the configured capture range and camera view.
4. The event cooldown had expired.
5. Events Core had time to finish the configured post-event period.
6. The reviewer has cctv.recording.view.

## Historic playback is empty or stops

Check the server console for recording finalization or storage errors. Confirm the Events Core data directory is writable and has available disk space. Test a new recording with the default capture range and timing before increasing limits.

## Map or MLO cameras are not found

Automatic discovery is disabled by default. After enabling it:

1. Enter the MLO so its objects are streamed.
2. Run /camerascan.
3. Run /cameraautoscanstatus.
4. Confirm the model is in both configured allowlists.
5. Verify the model-specific position and rotation offsets.

## An automatically discovered camera disappears after restart

Enable both CCTV persistence and persistDiscoveredCameras. If discovered-camera persistence is disabled, those records are intentionally temporary. Review repeated scan status before using either cleanup command.

## Settings or cameras reset after restart

Check that eventsCctv/data/cctv.json exists and can be written by the FXServer process. Restore a known-good backup only while the resource is stopped.

## Upload or Discord actions fail

Confirm the server-side integration is enabled and configured, then verify cctv.recording.upload or cctv.discord.send. Keep endpoints, tokens, and webhooks out of client files and redact them from support logs.

## Problems after an update

Replace both resources as complete packages, merge supported customer configuration into the new files, and start Events Core before CCTV. Do not mix protected runtime files from different versions. Restore data only after confirming it is compatible with the new release.

## Still need help?

Collect the relevant startup and error lines, installed resource versions, reproduction steps, and whether the issue affects live view, recording, or playback. Remove tokens, webhooks, player identifiers, and other private information before contacting [Sonoran Software support](https://support.sonoransoftware.com/).
