---
title: Developer API
published: true
date: 2026-08-17T00:00:00.000Z
tags: [fivem, cctv, lua exports, developer api]
editor: markdown
dateCreated: 2026-08-17T00:00:00.000Z
description: Use the supported Events CCTV server exports for remote viewing integrations.
---

# Developer API

Resource name:

```text
eventsCctv
```

CCTV currently exposes two server exports for trusted remote-view integrations. It does not expose client exports or public camera-management functions.

## Setup

Remote access is disabled by default. Enable it and allowlist each calling server resource in `eventsCctv/config/remote.lua`:

```lua
Config.RemotePanel = {
    Enabled = true,
    SessionTtlSeconds = 300,
    TrustedResources = {
        ['my-cctv-web-bridge'] = true
    }
}
```

The player must also be online and authorized for the requested terminal. The terminal must be enabled with `RemoteAccessEnabled = true`.

## Server exports

### `CreateRemoteSession(playerSource, terminalId)`

Creates a short-lived session for an authorized player and terminal.

```lua
local token, err = exports.eventsCctv:CreateRemoteSession(source, 'mission-row')

if not token then
    print(err)
end
```

| Parameter | Type | Description |
| --- | --- | --- |
| `playerSource` | number | Online player's server ID |
| `terminalId` | string | Existing CCTV terminal ID |

Returns `token, nil` on success or `nil, errorMessage` on failure. The token is a 48-character secret. Do not log it or send it to another player.

### `ValidateRemoteSession(token)`

Validates a session and returns the cameras available through its terminal.

```lua
local session, err = exports.eventsCctv:ValidateRemoteSession(token)

if session then
    print(session.terminalId, session.expiresAt, #session.cameras)
end
```

The result contains:

| Field | Type | Description |
| --- | --- | --- |
| `terminalId` | string | Authorized terminal ID |
| `cameras` | array | Enabled cameras assigned to the terminal |
| `expiresAt` | number | Session expiration as a Unix timestamp |

Returns `nil, errorMessage` when the token is expired, belongs to another calling resource, or access has been revoked. Validation re-checks the player, terminal, remote-access setting, and permissions each time.

## Security boundary

Both exports must be called from an allowlisted server resource. A token is bound to the resource that created it and cannot be validated by a different resource.

Events named `eventsCctv:server:*` and `eventsCctv:client:*` are internal request and synchronization routes. They are not a stable public API. Do not call them from integrations.
