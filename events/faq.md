---
title: FAQ
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, faq, events]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Frequently asked questions about the Events product suite.
---

# FAQ

## Does Events continuously record video?

No. Events captures structured gameplay state into bounded rolling buffers and
finalized sessions. Replay reconstructs that state. Add a trusted recorder worker
when an encoded MP4/WebM artifact is required.

## Can I install only one product?

Yes. `eventsCore` runs independently. `eventsCctv` and `eventsDashcam` are optional
DLC resources, and each requires the exact `eventsCore` resource name.

## Do I need ESX, QBCore, Qbox, SQL, or Node.js?

No. The default is standalone and does not require SQL or Node.js. Framework context
adapters are available when configured, and ACE remains the default permission
source.

## Do map cameras enroll automatically?

No. CCTV map-camera enrollment is disabled by default. It is opt-in, allowlist-based,
server-validated, and documented in [Automatic camera enrollment](events-cctv/automatic-camera-enrollment.md).

## Does the updater replace my files?

No. The Store version check is informational and `Config.Updater.EnableAutoUpdate`
is false by default.
Install updates from the official granted package, preserve supported config/data,
and restart in dependency order.

## Where are recordings stored?

Core-owned local artifacts use `eventsCore/data/recordings` by default. CCTV product
state uses `eventsCctv/data/cctv.json`. Review [Storage and retention](storage-and-retention.md)
before changing paths or backup policies.

## What are the Dashcam replay views?

Dashcam ships six validated views: front, rear, cabin, prisoner, left, and right.
They are structured reconstruction views, not six independently encoded video
streams.

## Is pricing or availability defined in this guide?

No. The current official Sonoran Store/Tebex product listing determines entitlement,
availability, and any commercial terms. This documentation describes the shipped
resource behavior only.
