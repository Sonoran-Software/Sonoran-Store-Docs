---
title: Events
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events, cctv, dashcam]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: The Events suite for structured CCTV and vehicle evidence capture, replay, storage, and integrations in FiveM.
---

# Events

Events is a three-resource FiveM suite for server-authoritative evidence workflows.
The free `eventsCore` package is the required foundation. CCTV is a separately
entitled paid DLC, while other DLC products use the same public core contracts.

| Resource | Version | Delivery | Role | Dependency |
| --- | --- | --- | --- | --- |
| `eventsCore` | `1.0.0` | Free foundation package | Provider-neutral capture, sessions, replay, processing, storage, retention, and integrations | None in the suite |
| `eventsCctv` | `1.0.0` | Paid DLC | CCTV terminals, cameras, live view, motion, and camera administration | `eventsCore` |
| `eventsDashcam` | `1.0.0` | Separate DLC; not part of the CCTV drop | Moving vehicle camera, trigger markers, evidence review, and six replay views | `eventsCore` |

The resources have a one-way ownership boundary. CCTV and Dashcam provide product
behavior and trigger intent; `eventsCore` owns the recording lifecycle and shared
evidence state. `eventsCctv` and `eventsDashcam` do not depend on one another.

## What the suite records

Events records structured gameplay state around a camera: entity identity and
transforms, health and movement state, environment data, event markers, and the
camera perspective used at the time. A session can preserve pre-event state,
append post-event state, and retain an immutable camera snapshot for later replay.

Replay is structured reconstruction, not a client screenshot stream or a native
video file. A trusted recorder worker is optional when an encoded MP4/WebM artifact
is required. Without one, sessions and replay plans still work and a requested
video job reports that no provider is available.

## Requirements

* A current FXServer with OneSync enabled.
* The official granted resource package and its Cfx.re Asset Escrow entitlement.
* The exact resource folders `eventsCore`, `eventsCctv`, and/or `eventsDashcam`.
* ACE configuration for the operators and reviewers who should use each product.

No framework, SQL database, Node.js runtime, or menu library is required by the
default configuration. The core has server-side context adapters for standalone,
ESX, QBCore, and Qbox deployments. CAD, Discord, external storage, and a recorder
worker are optional integrations.

## Start here

1. Follow [Installation](installation.md).
2. Grant the least-privilege nodes in [Permissions](permissions.md).
3. Set customer values in [Configuration](configuration.md).
4. Read the product guide for the installed DLC: [CCTV](events-cctv/README.md) or
   [Dashcam](events-dashcam/README.md).
5. Use [Storage and retention](storage-and-retention.md) and [Updating](updating.md)
   before enabling production evidence workflows.

## Distribution and support

Events is delivered through the official Sonoran Store/Tebex and Cfx.re flow.
Install the free Events Core package before any separately entitled paid DLC.
The current product listing determines the DLC price and availability. Do not
decrypt, modify, or redistribute protected runtime files.

For help, use [Sonoran Software support](https://support.sonoransoftware.com/).
