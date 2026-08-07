---
title: Dashcam troubleshooting
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events dashcam, troubleshooting]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Diagnose Dashcam eligibility, recording, replay, evidence, and integration issues.
---

# Dashcam troubleshooting

## The panel opens but recording is denied

Check `dashcam.use` and `dashcam.record`, then verify the server-observed driver,
vehicle model, plate/fleet policy, job context, inventory item, network ID, and
routing bucket. A client class-18 value is advisory and does not override a failed
server binding check.

## No automatic trigger appears

Confirm the trigger is enabled in `eventsDashcam/config/config.lua`, the player is
in an eligible vehicle, the polling loop can observe the event, and the cooldown
has elapsed. Automatic triggers add markers to an active core session; they do not
create a separate session when no valid session can be started.

## Replay has no video file

Replay is structured reconstruction. An encoded artifact requires a trusted recorder
worker registered and allowlisted in `eventsCore`. Check the core worker and storage
configuration rather than searching the Dashcam folder for a recording database.

## A user can record but cannot review evidence

Grant the appropriate `dashcam.review.own`, `dashcam.review.department`, or
`dashcam.review.all` node. Annotation, classification, lock, archive, and delete
are separate permissions.

## The camera duplicates after restart

Confirm only the Events Dashcam resource is registering the vehicle camera. Do not
run the legacy standalone capture engine for the same vehicle. Restart the core
before the DLC and inspect the server-observed vehicle binding.

For suite-wide dependency and live-environment checks, see [Events
Troubleshooting](../troubleshooting.md).
