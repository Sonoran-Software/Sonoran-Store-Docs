---
title: Persistence
published: true
date: 2026-07-27T00:00:00.000Z
tags: [json persistence, backup, display data]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Back up, restore, and understand the JSON persistence used by Sonoran Station Displays.
---

# Persistence

## Storage

Sonoran Station Displays uses JSON and has no database dependency.

| File | Purpose |
| --- | --- |
| `data/displays.DEFAULT.json` | Shipped empty template (`[]`) |
| `data/displays.json` | Generated runtime display data |

On first start, the server copies the default template to `data/displays.json`.

## When data is saved

The resource writes the full display array after:

* Creating a display
* Saving display settings
* Moving or renaming a display
* Deleting a display
* Changing a service filter through the server export

Runtime next/previous page changes and server `SetDisplayPage` broadcasts are not persisted.

## Display IDs

New IDs use:

```text
ssd-<eight hexadecimal time characters>-<six random hexadecimal characters>
```

Example:

```text
ssd-67a12f30-09bcde
```

Treat IDs as opaque strings. Do not derive authorization or location from an ID.

## Restart behavior

On resource start:

1. The JSON file is decoded.
2. Every display is run through current validation.
3. Valid entries load with their original ID and timestamps.
4. Invalid entries are skipped with `SSD-PERSIST-002`.
5. Authorized clients receive the loaded set when they request synchronization.

There is no hot-reload command for the persistence file.

## Backup

Before an update or major display change:

1. Stop `sonoran-stationdisplay`.
2. Copy `data/displays.json` to a safe backup location.
3. Back up `config.lua` and any edited locale files.
4. Perform the update.
5. Restore or migrate the saved files as directed by the release notes.
6. Start the resource and review the loaded display count.

Do not edit generated display data while the server is running. A later in-game save can overwrite manual changes.

## Manual editing

Manual editing is not a supported customer workflow. The server validates the file, but a syntactically valid change can still move a display, select an invalid model, reset values to defaults, or make an entry fail loading.

Use the management menu or supported server exports. If support asks for the file, remove player identifiers or other server-specific information before sharing it.

## Corrupt data

| Code | Meaning | Recovery |
| --- | --- | --- |
| `SSD-PERSIST-001` | The JSON document could not be decoded or was not a table. | Stop the resource, preserve the bad file for diagnostics, then restore a backup or replace it with `[]`. |
| `SSD-PERSIST-002` | One entry failed display validation. | Review the logged index, restore that entry from backup, or recreate it in-game. Other valid entries still load. |
| `SSD-PERSIST-003` | The display array could not be JSON encoded. | Preserve console logs and the current file, then contact support. |

The resource does not automatically rewrite a corrupt file or run a database migration. Current validation acts as the version 1.0.0 load-time migration boundary.

