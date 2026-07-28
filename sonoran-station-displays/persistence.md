---
title: Persistence
published: true
date: 2026-07-28T00:00:00.000Z
tags: [json persistence, backup, display data]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Understand verified JSON persistence, backup, restore, and console repair.
---

# Persistence

Station Displays uses JSON and has no database dependency.

| File | Purpose |
| --- | --- |
| `data/displays.DEFAULT.json` | Shipped template, normally `[]` |
| `data/displays.json` | Generated active runtime data |

## First start

Only when `data/displays.json` is missing, the server:

1. Reads and validates the default as a JSON array.
2. Writes it to the active path.
3. Reads the active path back.
4. Verifies that it is a decodable array.

An existing active file is never silently overwritten by the default. If initialization finds an invalid existing file, it leaves it in place and reports `SSD-PERSIST-INIT-005`.

## Saves

The complete normalized display array is saved after create, settings/name/transform changes, delete, and the server `SetDisplayServiceFilter` export.

Runtime page changes, nearby viewer controls, map pan, bodycam feed state, preview state, and `SetDisplayPage` are not persisted.

Because the underlying write function does not provide a reliable success result on every runtime, the resource treats readback as authoritative. A save succeeds only after the file is non-empty, decodes as an array, and exactly matches the normalized state that was written.

`SSD-PERSIST-003` reports one of these reasons:

```text
ENCODE_FAILED
WRITE_FAILED
READBACK_FAILED
READBACK_INVALID
READBACK_MISMATCH
```

Client synchronization occurs after persistence. If the file was saved but a broadcast fails, the audit reports `CLIENT_BROADCAST_FAILED`; that is not a persistence failure.

## Restart behavior

At startup, each decoded record is normalized and validated. Valid entries keep their opaque ID/timestamps; invalid entries are skipped with `SSD-PERSIST-002`. There is no explicit `schemaVersion` field in the persisted array and no hot-reload command.

Defaults in `config.lua` do not overwrite valid saved per-display values. Current normalization provides the load-time compatibility boundary, including legacy theme fallback.

## Backup and restore

1. Stop `sonoran-stationdisplay`.
2. Copy `data/displays.json`, `config.lua`, locale changes, and server webhook configuration to a protected backup.
3. Replace/update the resource as directed.
4. Merge customer configuration into the new schema.
5. Restore `data/displays.json`.
6. Start the resource and confirm the loaded display count.

Do not edit the active file while the resource is running; the next supported save can overwrite it. Manual editing is not a supported workflow because a syntactically valid change can still fail model, enum, page, range, or world-context validation.

## Console repair

When the active file is unreadable and no good backup is available, run from the server console:

```text
stationdisplay_repair_persistence
```

The command is console-only. It copies the invalid file to:

```text
data/displays.invalid.<epoch>.json
```

then restores the validated default. Restart the resource afterward. The backup can contain server locations and other operational data; protect it accordingly.

## Codes

| Code | Meaning |
| --- | --- |
| `SSD-PERSIST-INIT-*` | Default/active file initialization failure; `005` means an invalid existing active file was preserved. |
| `SSD-PERSIST-001` | Active JSON cannot be decoded as the required array. |
| `SSD-PERSIST-002` | One saved entry failed validation and was skipped. |
| `SSD-PERSIST-003` | Encode, write, readback, validation, or exact-state verification failed. |

Use the in-game/server [Diagnostics](diagnostics.md) snapshot and local audit ID when contacting support.
