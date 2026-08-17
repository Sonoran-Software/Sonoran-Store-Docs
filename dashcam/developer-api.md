---
title: Developer API
published: true
date: 2026-08-17T00:00:00.000Z
tags: [fivem, dashcam, lua exports, events, developer api]
editor: markdown
dateCreated: 2026-08-17T00:00:00.000Z
description: Use the supported Events Dashcam client and server exports and integration events.
---

# Developer API

Resource name:

```text
eventsDashcam
```

## Client exports

| Export | Returns | Description |
| --- | --- | --- |
| `StartDashcamRecording(trigger)` | boolean | Submits a recording request for the local driver. Defaults to `manual`. |
| `StopDashcamRecording()` | boolean | Requests finalization of the local driver's active recording. |
| `SaveDashcamBuffer()` | boolean | Starts a manual save with the configured pre-event buffer. |
| `AddDashcamMarker(label, markerType)` | boolean | Adds a marker to the active recording. Defaults to `Manual evidence marker` and `manual`. |
| `IsDashcamRecording()` | boolean | Returns the local recording state. |
| `GetActiveRecordingId()` | string or nil | Returns the local active recording ID. |

```lua
local accepted = exports.eventsDashcam:StartDashcamRecording('manual')

if exports.eventsDashcam:IsDashcamRecording() then
    exports.eventsDashcam:AddDashcamMarker('Suspect vehicle located', 'observation')
end
```

An `accepted` result only means the client submitted the request. The server still checks the player's permissions, vehicle, trigger configuration, rate limits, and current recording state.

Supported trigger names are `manual`, `save_buffer`, `emergency_lights`, `siren`, `collision`, `speed`, `gunshot`, `dispatch`, and `marker`.

## Server exports

Server exports that act for a player require that player's server ID. They re-check access and evidence permissions.

### Read a recording

```lua
local recording, err = exports.eventsDashcam:GetDashcamRecording(recordingId, source)
local canAccess = exports.eventsDashcam:CanAccessDashcamRecording(source, recordingId)
```

| Export | Returns | Description |
| --- | --- | --- |
| `GetDashcamRecording(recordingId, playerSource)` | recording or nil, error or nil | Returns the recording when the player can access it. |
| `CanAccessDashcamRecording(playerSource, recordingId)` | boolean | Checks whether the player can access the recording. |

A server resource may omit `playerSource` only when its resource name is explicitly allowlisted in `Config.Permissions.TrustedResources` inside `eventsDashcam/config/permissions.lua`:

```lua
Config.Permissions.TrustedResources = {
    ['my-evidence-bridge'] = true
}

local recording, err = exports.eventsDashcam:GetDashcamRecording(recordingId)
```

Do not use the trusted form for player-initiated requests. Pass the player's source so Dashcam applies their permissions.

### Update a recording

```lua
local ok, err = exports.eventsDashcam:AttachIncidentToRecording(source, recordingId, 'INC-1042')
local ok, err = exports.eventsDashcam:LockDashcamRecording(source, recordingId, 'Evidence hold')
local ok, err = exports.eventsDashcam:AddDashcamEvidenceNote(source, recordingId, 'Reviewed by supervisor')
```

| Export | Returns | Description |
| --- | --- | --- |
| `AttachIncidentToRecording(playerSource, recordingId, incident)` | boolean, error or nil | Sets the incident number. Maximum 128 characters. |
| `LockDashcamRecording(playerSource, recordingId, reason)` | boolean, error or nil | Locks the recording. Defaults the reason to `external lock` when omitted. |
| `AddDashcamEvidenceNote(playerSource, recordingId, body)` | boolean, error or nil | Adds an evidence note. Maximum 4,096 characters. |

These calls fail when the recording is missing, inaccessible, locked where applicable, or the player lacks the required permission.

## Integration events

### Start from another client resource

Use the local client event when an integration needs to request a configured trigger:

```lua
TriggerEvent('dashcam:integration:trigger', 'dispatch', {
    incident = 'INC-1042'
})
```

The server remains authoritative. Incident text is limited to 128 characters.

### Observe recording lifecycle

```lua
AddEventHandler('dashcam:client:recordingStarted', function(recordingId)
    -- Runs on the recording player's client.
end)

AddEventHandler('dashcam:client:recordingStopped', function(recordingId)
    -- Runs on the recording player's client.
end)

AddEventHandler('dashcam:server:recordingFinalized', function(recordingId, recording)
    -- Runs on the server after Events Core finalizes a Dashcam recording.
end)
```

The server event includes a defensive recording table when it is available. Treat it as read-only.

## Internal routes

Events named `eventsDashcam:server:*` and `eventsDashcam:client:*` are internal request and synchronization routes. They are not a stable public API. Use the exports and integration events above instead.
