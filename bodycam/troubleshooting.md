---
title: Troubleshooting
published: true
date: 2026-08-18T00:00:00.000Z
tags: [fivem, bodycam, troubleshooting]
editor: markdown
dateCreated: 2026-08-18T00:00:00.000Z
description: Resolve common Bodycam installation, eligibility, recording, permission, replay, and evidence problems.
---

# Bodycam troubleshooting

Begin with the FXServer console and confirm both resources are current. Enable debug output only temporarily on a test server.

## Bodycam will not start

Confirm the exact folder names and start order:

~~~cfg
ensure eventsCore
ensure eventsBodycam
~~~

Each fxmanifest.lua must be directly inside its resource folder. Bodycam requires OneSync and a compatible Events Core version. Review the first eventsBodycam startup error instead of only later follow-on messages.

If EUP eligibility is enabled, at least one valid rule and drawable are required. Invalid wearer, camera, retention, permission, or trigger settings also block startup.

## F6 does not open the terminal

Confirm eventsBodycam is running, the configured Panel key is registered, and no other resource has taken the same key. Try /bodycam. If the command works, review the player's FiveM key bindings.

## The terminal opens but the evidence list is empty

The list includes only recordings the player may review. Grant bodycam.review.own, bodycam.review.department, or bodycam.review.all as appropriate, then confirm the recording has finalized. Framework users must be on duty when their job context requires it.

## Recording is denied

Check all of the following:

* The player has bodycam.record through ACE or the configured framework grade
* The player passes the configured `permission`, `any`, or `all` wearer policy
* The server can resolve a networked player ped
* The framework does not report the wearer off duty when RequireOnDuty is enabled
* Events Core is running and Bodycam completed startup
* The trigger and request cooldowns have expired

Ped changes, respawns, routing-bucket changes, and stale wearer bindings are revalidated server-side.

## EUP eligibility does not work

Verify the component slot, drawable ID, and optional texture IDs against the actual ped variation. An empty texture table allows every texture for an approved drawable.

If the server console reports that the EUP integration is unavailable, your runtime cannot provide the needed server variation natives. Connect eventsBodycam/integrations/eup.lua to a trusted server-side clothing resource. Do not send the component state from the client.

## Inventory eligibility does not work

The shipped inventory adapter returns false until configured. Confirm the item name and implement `HasBodycamItem` in eventsBodycam/integrations/inventory.lua using your inventory's server API. Then verify that the selected eligibility mode actually uses the inventory result.

## An automatic trigger does not start

Confirm the trigger is enabled in eventsBodycam/config/config.lua and the player is an eligible wearer with bodycam.record. For damage, verify the observed health loss reaches DamageDelta. For officer down, verify health reaches OfficerDownHealth or the ped dies. Panic can be tested with /bodycampanic.

If a recording is already active, the trigger is added to that recording's timeline instead of creating another recording.

## A marker is not added

Markers require an active recording, access to that recording, and bodycam.annotate or a qualifying framework job grade. Wait for the marker cooldown before trying again. Bodycam does not assign a marker key by default; use /bodycammark or configure `Config.Keys.Marker`.

## A user can record but cannot review

Recording and review are separate permissions. Grant review.own for personal evidence, review.department for matching department evidence, or review.all for broad review. Department access also requires the current framework department to match the department stored on the recording.

## Replay does not show a video file

This is expected for the standard workflow. Bodycam uses structured in-game replay rather than a conventional video file. Select a recording and choose **Open Structured Replay**. Encoded video generation or upload requires a separately configured compatible processing setup.

## Replay is empty or stops

Check the Events Core console for finalization, storage, reconstruction, or permission errors. Confirm eventsCore/data/recordings is writable and has available disk space. Create a new short recording with the shipped capture settings before increasing limits or testing custom integrations.

## The recorded angle is positioned incorrectly

Custom peds, clothing, and EUP packs can place the configured bone differently. Adjust the bone, position offset, rotation offset, or field of view in eventsBodycam/config/cameras.lua on a test server. Verify the result with every supported ped and uniform.

## Classification or custody actions are denied

Evidence access alone does not grant every action. Check bodycam.annotate, bodycam.classify, bodycam.archive, bodycam.lock, and bodycam.delete separately. Locked records cannot be edited or deleted, and preserved records cannot be deleted.

## Retention is not what you expected

Changing classification applies the matching value from eventsBodycam/config/retention.lua. A value of 0 means no automatic expiry. To change retention manually, confirm the reviewer has bodycam.archive and the requested value is between 0 and the recording's maximum of 3,650 days.

## Problems after an update

Replace both resources as complete packages, merge supported customer configuration into the new files, and start Events Core before Bodycam. Do not mix protected runtime files from different versions or restore old configuration without comparing its schema.

## Still need help?

Collect the relevant startup and error lines, installed resource versions, reproduction steps, wearer policy and permission method, ped/uniform details when relevant, and the trigger or evidence action that failed. Remove tokens, webhooks, player identifiers, and other private information before contacting [Sonoran Software support](https://support.sonoransoftware.com/).
