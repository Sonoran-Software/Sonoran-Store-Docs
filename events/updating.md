---
title: Updating
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, update, asset escrow, events]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Safely update the Events resources while preserving configuration, evidence data, and the dependency order.
---

# Updating

Events includes a Store version check for each resource. It is informational: the
check does not download, unzip, overwrite, or mutate a customer installation.

## Before updating

1. Read the resource changelog and record the installed version from its
   `fxmanifest.lua` or startup output.
2. Confirm the target package contains the same exact resource names:
   `eventsCore`, `eventsCctv`, and `eventsDashcam`.
3. Stop the Events resources or stop the server before replacing files.
4. Back up supported configuration and runtime data:

```text
eventsCore/config/
eventsCore/data/
eventsCctv/config/
eventsCctv/data/cctv.json
eventsDashcam/config/
```

Backups can contain sensitive evidence metadata. Store them securely and do not
share them with support unless requested through the support process.

## Replace the package

1. Download the current granted asset through the official CFX/Tebex delivery flow.
2. Extract it to a temporary location and verify each `fxmanifest.lua` is at the
   resource root.
3. Replace the complete resource folder; do not copy only protected Lua files into
   an older install.
4. Merge customer configuration into the new shipped schema instead of blindly
   replacing the new config with an old copy.
5. Restore data files only when the release changelog says they remain compatible.
6. Do not decrypt, merge, or edit protected Asset Escrow files.

## Restart order

```cfg
ensure eventsCore
ensure eventsCctv
ensure eventsDashcam
```

Restart the full server when practical so connected clients do not retain an older
NUI or replay state. If only a DLC is updated, restart `eventsCore` first when the
core version changed, then restart the DLCs.

## Version-check behavior

The protected stable-channel check starts with a stagger (`eventsCore` 3 seconds,
`eventsCctv` 11 seconds, `eventsDashcam` 19 seconds) and repeats every six hours.
The release endpoint is protected by the package and is not a customer-editable
runtime URL. `Config.Updater.EnableAutoUpdate = false` is the only shipped updater
setting. A newer or incompatible version produces an operator-facing log message;
it does not change files.

The DLCs fail closed when `eventsCore` is missing or below the configured minimum
version. Update the core and installed DLCs as a compatible set, then restart them
in dependency order.

## Verify after updating

* Confirm all installed resources report the expected version.
* Confirm the core starts before the DLCs and no compatibility warning remains.
* Open the CCTV and/or Dashcam interface with an authorized test account.
* Review a pre-update test session and create a new test session.
* Confirm persistence files load and integrations remain disabled or configured as
  intended.

For update failures, see [Troubleshooting](troubleshooting.md) and contact
[Sonoran Software support](https://support.sonoransoftware.com/).
