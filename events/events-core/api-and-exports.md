---
title: Core API and exports
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events core, api, exports]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Public server exports for Events providers, sessions, replay, workers, evidence, and integrations.
---

# Core API and exports

The public core contract is server-side. A provider must register using its exact
resource name; core ownership checks prevent a provider from reading or mutating a
different provider's cameras or sessions.

## Provider and camera exports

```text
RegisterProvider             UnregisterProvider
RegisterCamera               UpdateCamera
UnregisterCamera             GetCamera
GetProviderCameras           GetCameraBufferStatus
WasEntityVisibleToCamera
```

Use a world, entity, or bone transform. Dynamic cameras must provide a server-safe
networked entity binding; the core marks an approximate bone state explicitly when
an exact runtime resolver is not available.

## Session and processing exports

```text
RegisterEventType            TriggerCameraEvent
TriggerEventForCameras       CreateManualRecording
StartManualRecording         FinalizeRecording
GetActiveRecording           ExtendRecording
CancelRecording              GetRecordingSession
ListRecordingSessions        ListProviderSessions
RequestReconstruction        RequestInteractiveReconstruction
ControlInteractiveReconstruction
StopInteractiveReconstruction
RequestVideoGeneration       GetProcessingJob
RegisterWorkerProvider
```

Interactive replay requires the source-owned token and validated control actions.
Automatic events do not select a player's client as a reconstruction worker.

## Evidence and integration exports

```text
AddRecordingMarker           AddRecordingNote
UpdateRecordingMetadata      SetRecordingLocked
SetRecordingPreserved        SetRecordingArchived
SetRecordingRetention        PreserveRecording
ReleaseRecording             UnlockRecording
ArchiveRecording             DeleteRecording
UploadRecording              SendRecordingToCAD
SendRecordingToDiscord
```

The core also exposes server-side player context and permission helpers:

```lua
local context = exports['eventsCore']:GetPlayerContext(source)
local allowed = exports['eventsCore']:HasPermission(source, 'my_dlc.recording.view', {
    provider = 'my_dlc',
    cameraId = cameraId
})
```

Keep worker names, export names, and integration secrets server-only. Product
resources should use these public exports instead of calling core internals.
