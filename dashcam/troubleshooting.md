---
title: Troubleshooting
published: true
date: 2026-08-12T00:00:00.000Z
tags: [fivem, dashcam, troubleshooting]
editor: markdown
dateCreated: 2026-08-12T00:00:00.000Z
description: Resolve common Dashcam installation, eligibility, recording, permission, replay, and evidence problems.
---

# Dashcam troubleshooting

Begin with the FXServer console and confirm both resources are current. Enable debug output only temporarily on a test server.

## Dashcam will not start

Confirm the exact folder names and start order:

~~~cfg
ensure eventsCore
ensure eventsDashcam
~~~

Each fxmanifest.lua must be directly inside its resource folder. Dashcam requires OneSync and a compatible Events Core version. Review the first eventsDashcam startup error instead of only the later follow-on messages.

## F7 does not open the terminal

Confirm eventsDashcam is running, the configured Panel key is registered, and no other resource has taken the same key. Try /dashcam. If the command works, review the player's FiveM key bindings.

## The terminal opens but the evidence list is empty

The list only includes recordings the player may review. Grant the appropriate dashcam.review.own, dashcam.review.department, or dashcam.review.all permission, then confirm the recording has finalized. Framework users must also be on duty when their job context requires it.

## Recording is denied

Check all of the following:

* The driver has dashcam.record through ACE or the configured framework grade
* The player is in the driver's seat
* The vehicle is networked and eligible by class, model, plate, job, fleet, or inventory policy
* Events Core is running and Dashcam completed startup
* The trigger and request cooldowns have expired

Passenger requests and stale vehicle bindings are intentionally denied.

## An automatic trigger does not start

Confirm the trigger is enabled in eventsDashcam/config/config.lua and the driver is in an eligible vehicle. For collisions, verify the damage change exceeds CollisionDamageDelta. For speeding, remember SpeedMps uses metres per second. For gunshots, verify the event occurred within GunshotRadius.

If a recording is already active, the trigger is added to that recording's timeline instead of creating another recording.

## F10 does not add a marker

Markers require an active recording, access to that recording, and dashcam.annotate or a qualifying framework job grade. Wait for the marker cooldown before trying again.

## A user can record but cannot review

Recording and review are separate permissions. Grant review.own for personal evidence, review.department for matching department evidence, or review.all for broad review. Department access also requires the current framework department to match the department stored on the recording.

## Replay does not show a video file

This is expected for the standard workflow. Dashcam uses structured in-game replay rather than a conventional video file. Select a recording and choose **Open Structured Replay**. Encoded video generation or upload requires a separately configured compatible processing setup.

## Replay is empty or stops

Check the Events Core console for finalization, storage, reconstruction, or permission errors. Confirm eventsCore/data/recordings is writable and has available disk space. Create a new short recording with the shipped capture settings before increasing limits or testing custom integrations.

## A replay view is positioned incorrectly

Vehicle layouts and bone positions can vary between custom models. Adjust the matching view in eventsDashcam/config/cameras.lua on a test server, then verify every view on every supported vehicle model. Keep all six unique view IDs.

## Classification or custody actions are denied

Evidence access alone does not grant every action. Check dashcam.annotate, dashcam.classify, dashcam.archive, dashcam.lock, and dashcam.delete separately. Locked records cannot be edited or deleted, and protected records cannot be deleted.

## Retention is not what you expected

Changing classification applies the matching value from eventsDashcam/config/retention.lua. A value of 0 means no automatic expiry. To change retention manually, confirm the reviewer has dashcam.archive and the requested value is between 0 and 3,650 days.

## Problems after an update

Replace both resources as complete packages, merge supported customer configuration into the new files, and start Events Core before Dashcam. Do not mix protected runtime files from different versions or restore old configuration without comparing its schema.

## Still need help?

Collect the relevant startup and error lines, installed resource versions, reproduction steps, vehicle model and plate, permission method, and the trigger or evidence action that failed. Remove tokens, webhooks, player identifiers, and other private information before contacting [Sonoran Software support](https://support.sonoransoftware.com/).
