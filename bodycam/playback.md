---
title: Playback and Evidence
published: true
date: 2026-08-18T00:00:00.000Z
tags: [fivem, bodycam, playback, evidence, retention]
editor: markdown
dateCreated: 2026-08-18T00:00:00.000Z
description: Search Bodycam evidence, replay the recorded perspective, and manage notes, classifications, locks, archives, and retention.
---

# Bodycam playback and evidence

Finalized recordings appear in the Body-Worn Camera Evidence terminal for authorized reviewers. Press F6 or run /bodycamreview, then search or filter the evidence list and select a record.

## Find a recording

The terminal can search the information attached to accessible evidence, including:

* Recording ID
* Officer name or callsign
* Wearer model
* Department
* Incident or case number
* Citation number
* Arrest-report number

You can also filter by classification and finalized state. The result list is permission-filtered, so users see only their own, departmental, or all Bodycam evidence according to their access.

## Recorded bodycam perspective

Every Bodycam recording retains one body-worn camera definition. The shipped view is attached to the upper-spine bone with a 78-degree field of view. The recording keeps its captured perspective, so a reviewer can return after the original wearer or scene is gone.

When exact animated bone coordinates were unavailable to the server during capture, the recorded transform may be marked approximate. Verify camera placement with the peds and uniforms used by your server.

## Playback controls

Open **Structured Replay** from the selected record. The terminal supports:

* Play and pause
* Restart
* Seek with the timeline
* Skip backward or forward 10 seconds
* Step backward or forward
* Playback speeds of 0.25x, 0.5x, 1x, 2x, and 4x
* Exiting replay and returning to the evidence terminal

Bodycam recreates the incident inside FiveM from recorded gameplay state. A normal Bodycam replay is not an MP4 or continuous screen recording. Separate video generation or upload requires an optional compatible processing setup.

## Evidence details

Depending on permissions, reviewers can:

* Set the classification
* Add or update incident, case, citation, and arrest-report numbers
* Add operational notes
* Lock or unlock the record
* Archive or unarchive the record
* Change retention days
* Delete an unlocked, unpreserved record with a substantive reason

Locked records cannot be edited or deleted until an authorized reviewer unlocks them. Preserved records cannot be deleted. Deletion is permanent and requires bodycam.delete.

## Shipped classifications and retention

| Classification | Retention |
| --- | ---: |
| routine | 30 days |
| traffic_stop | 90 days |
| arrest | 180 days |
| use_of_force | 365 days |
| officer_involved_shooting | No automatic expiry |
| complaint | 365 days |
| training | 30 days |
| indefinite | No automatic expiry |

Changing a record's classification applies that classification's configured retention policy. A retention value of 0 means the record has no automatic expiry. Bodycam accepts configured and manual values from 0 through the camera policy maximum of 3,650 days.

Review your local evidence policy and available storage before changing retention. Record data is stored under eventsCore/data/recordings by default; back it up securely before updates or storage changes.
