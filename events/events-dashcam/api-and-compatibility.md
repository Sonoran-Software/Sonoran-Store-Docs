---
title: Dashcam API and compatibility
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events dashcam, api, integrations]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Public Dashcam exports, compatibility events, replay controls, and migration boundaries.
---

# Dashcam API and compatibility

All routes validate untrusted payloads and call provider-owned core exports.

## Client exports

```text
StartDashcamRecording(trigger)
StopDashcamRecording()
SaveDashcamBuffer()
AddDashcamMarker(label, type)
IsDashcamRecording()
GetActiveRecordingId()
```

## Server exports

```lua
exports.eventsDashcam:GetDashcamRecording(recordingId, source)
exports.eventsDashcam:GetDashcamRecording(recordingId) -- trusted callers only
exports.eventsDashcam:AttachIncidentToRecording(source, recordingId, incident)
exports.eventsDashcam:LockDashcamRecording(source, recordingId, reason)
exports.eventsDashcam:AddDashcamEvidenceNote(source, recordingId, body)
exports.eventsDashcam:CanAccessDashcamRecording(source, recordingId)
```

The one-argument compatibility form requires the invoking resource to be listed in
the server-only trusted-resource allowlist. Mutating exports always take a source.

## Compatibility boundary

Lifecycle notifications and product trigger names remain available where they do
not imply media transport. Audio/radio events carry metadata only. The legacy
client sampler, chunk upload/token/ack protocol, direct recording-file storage,
native voice capture, and legacy media URL replay are intentionally unsupported.

Do not run both capture engines for one vehicle. See [Vehicle setup](vehicle-setup.md)
and [Playback](playback.md).
