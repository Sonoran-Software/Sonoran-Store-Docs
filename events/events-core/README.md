---
title: Events Core
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events core, recording, replay]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Configure the provider-neutral recording, replay, storage, and integration foundation for Events.
---

# Events Core

`eventsCore` is the required foundation for the Events DLC resources. It owns the
shared lifecycle:

* provider and camera registration;
* rolling state buffers and event sessions;
* pre-roll/post-roll finalization and immutable camera snapshots;
* structured reconstruction and interactive replay;
* worker queues, retries, storage, retention, audit, CAD, and Discord contracts.

The core does not require CCTV or Dashcam and can start independently. It does not
continuously encode video on its own.

## Start order

```cfg
ensure eventsCore
```

If CCTV or Dashcam is installed, start those resources after the core. The DLCs use
the public core exports and do not access core implementation files.

## Worker model

Media encoding, uploads, and optional reconstruction workers are replaceable server
resources. A worker must be registered through the core contract and allowlisted in
`Config.Permissions.WorkerResources`. Automatic player workers are disabled by
default.

See [Storage and retention](storage-and-retention.md) for artifact behavior and
[API and exports](api-and-exports.md) for integration boundaries.
