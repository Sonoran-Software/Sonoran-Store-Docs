---
title: Troubleshooting
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, troubleshooting, events]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Diagnose common Events startup, permission, recording, storage, and integration problems.
---

# Troubleshooting

Start with the server console and the resource-specific configuration. Keep
`Config.Debug = false` during normal operation; enable it briefly on a test server
when a documented workflow needs more detail.

## A DLC will not start

Confirm the exact order:

```cfg
ensure eventsCore
ensure eventsCctv
ensure eventsDashcam
```

Confirm the folder is named exactly and that the core resource is running. CCTV
and Dashcam intentionally block provider registration when the core is unavailable
or below the configured minimum version. Update the core and restart the DLC after
the core is ready.

## The interface opens but actions are denied

Check the ACE node for the requested operation, the player group membership, and
the resource's server-only permission config. For CCTV, also check terminal group
or terminal-specific nodes. For Dashcam, check both `dashcam.use` and the more
specific review/record/evidence node.

The UI is not the authority. A successful browser request cannot bypass server
ownership, routing-bucket, distance, rate, or evidence-state checks.

## A recording has no video

Events records structured state by default. An encoded artifact requires a trusted
recorder worker registered with `eventsCore` and allowlisted in
`Config.Permissions.WorkerResources`. If no worker is registered, the session and
structured replay can remain valid while a video request reports
`provider_unavailable`.

## Cameras are missing

For configured cameras, check `eventsCctv/data/cctv.json`, camera enabled state,
terminal scope, routing bucket, and the camera permission node. For map cameras,
remember that automatic enrollment is disabled by default and only scans bounded
areas after the object is streamed. Use `/camerascan` after entering an MLO and
check the model allowlist and per-model offsets.

## Storage or retention behaves unexpectedly

Check the core storage root, MIME type, per-file limit, total limit, retention
classification, and preserve/lock state. A file-size or host rejection does not
invalidate a structured session. Review the [storage guide](storage-and-retention.md)
before deleting data.

## CAD or Discord does not send

Both integrations are disabled by default. Verify the server-only enabled flag,
endpoint/webhook, token, and request permissions. Test with a non-production target
first. The core retries bounded failures and does not send credentials to the NUI.

## What repository tests do not prove

The source test suite cannot prove licensed FXServer startup, OneSync entity/native
behavior, GTA render fidelity, Cfx.re Asset Escrow entitlement, a real recorder,
target filesystem permissions, or the configured CAD/Discord endpoints. Complete
the target-server smoke test before launch and provide the relevant console output
when requesting support.
