---
title: Developer API
published: true
date: 2026-08-18T00:00:00.000Z
tags: [fivem, bodycam, lua exports, events, developer api]
editor: markdown
dateCreated: 2026-08-18T00:00:00.000Z
description: Use the supported Events Bodycam client and server exports and integration events.
---

# Developer API

Resource name:

```text
eventsBodycam
```

All player requests remain subject to the server's wearer binding, trigger policy, permissions, access checks, rate limits, and payload limits.

## Client exports

| Export | Returns | Description |
| --- | --- | --- |
| `StartBodycamRecording(trigger)` | boolean | Submits a recording request for the local wearer. Defaults to `manual`. |
| `StopBodycamRecording()` | boolean | Requests finalization of the local wearer's active recording. |
| `SaveBodycamBuffer()` | boolean | Starts a manual save with the configured pre-event buffer. |
| `AddBodycamMarker(label, markerType)` | boolean | Adds a marker to the active recording. Defaults to `Manual evidence marker` and `manual`. |
| `IsBodycamRecording()` | boolean | Returns the local recording state. |
| `GetActiveRecordingId()` | string or nil | Returns the local active recording ID. |

```lua
local accepted = exports.eventsBodycam:StartBodycamRecording('manual')

if exports.eventsBodycam:IsBodycamRecording() then
    exports.eventsBodycam:AddBodycamMarker('Contact initiated', 'observation')
end
```

An `accepted` result only means the client submitted the request. The server still checks eligibility, permissions, the server-observed ped binding, trigger configuration, cooldowns, and current recording state.

Supported trigger names are `manual`, `save_buffer`, `gunshot`, `damage`, `officer_down`, `panic`, and `dispatch`.

## Server exports

Server exports that act for a player require that player's server ID. They re-check access and evidence permissions.

### Read a recording

```lua
local recording, err = exports.eventsBodycam:GetBodycamRecording(recordingId, source)
local canAccess = exports.eventsBodycam:CanAccessBodycamRecording(source, recordingId)
```

| Export | Returns | Description |
| --- | --- | --- |
| `GetBodycamRecording(recordingId, playerSource)` | recording or nil, error or nil | Returns the recording when the player can access it. |
| `CanAccessBodycamRecording(playerSource, recordingId)` | boolean | Checks whether the player can access the recording. |

A server resource may omit `playerSource` only when its resource name is explicitly allowlisted in `Config.Permissions.TrustedResources` inside eventsBodycam/config/permissions.lua:

```lua
Config.Permissions.TrustedResources = {
    ['my-evidence-bridge'] = true
}

local recording, err = exports.eventsBodycam:GetBodycamRecording(recordingId)
```

Do not use the trusted form for player-initiated requests. Pass the player's source so Bodycam applies their permissions.

### Update a recording

```lua
local ok, err = exports.eventsBodycam:AttachIncidentToRecording(source, recordingId, 'INC-1042')
local ok, err = exports.eventsBodycam:LockBodycamRecording(source, recordingId, 'Evidence hold')
local ok, err = exports.eventsBodycam:AddBodycamEvidenceNote(source, recordingId, 'Reviewed by supervisor')
```

| Export | Returns | Description |
| --- | --- | --- |
| `AttachIncidentToRecording(playerSource, recordingId, incident)` | boolean, error or nil | Sets the incident number. Maximum 128 characters. |
| `LockBodycamRecording(playerSource, recordingId, reason)` | boolean, error or nil | Locks the recording. Defaults the reason to `external lock` when omitted. |
| `AddBodycamEvidenceNote(playerSource, recordingId, body)` | boolean, error or nil | Adds an evidence note. Maximum 4,096 characters. |

These calls fail when the recording is missing, inaccessible, locked where applicable, or the player lacks the required permission.

## Integration events

### Start from another client resource

Use the local client event when an integration needs to request a supported trigger:

```lua
TriggerEvent('bodycam:integration:trigger', 'dispatch', {
    incident = 'INC-1042'
})
```

The server remains authoritative. Incident text is limited to 128 characters.

### Attach voice or radio metadata

Bodycam accepts only the documented metadata event types: `voice_activity`, `radio_tx`, and `radio_channel`.

```lua
TriggerEvent('bodycam:client:audioEvent', 'radio_tx', {
    channel = 1,
    transmitting = true
})
```

This adds bounded event metadata to an active recording. Do not send audio bytes, media chunks, or credentials.

### Observe recording lifecycle

```lua
AddEventHandler('bodycam:client:recordingStarted', function(recordingId)
    -- Runs on the recording player's client.
end)

AddEventHandler('bodycam:client:recordingStopped', function(recordingId)
    -- Runs on the recording player's client.
end)

AddEventHandler('bodycam:server:recordingFinalized', function(recordingId, recording)
    -- Runs on the server after Events Core finalizes a Bodycam recording.
end)
```

The server event includes a defensive recording table when it is available. Treat it as read-only.

## Security boundary

No Bodycam API accepts media chunks, native audio bytes, screenshots, upload tokens, client-selected peds, file paths, or arbitrary recording URLs. Capture, reconstruction, storage, retention, and processing remain owned by Events Core.

Events named `eventsBodycam:server:*` and `eventsBodycam:client:*` are internal request and synchronization routes. They are not a stable public API. Use the exports and integration events above instead.
