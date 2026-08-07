---
title: Installation
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, installation, events]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Install Events Core, CCTV, and Dashcam with the correct resource names, start order, and first boot checks.
---

# Installation

## Before you begin

You need a current FXServer with OneSync enabled, access to the CFX account that
owns the granted package, and permission to add resources and ACE entries to the
server. Use a test server first when enabling a recorder, CAD, Discord, or external
storage integration.

Events is standalone by default. ESX, QBCore, Qbox, and optional integration
resources can be configured after the base installation.

## 1. Download the granted packages

Download the free Events Core package and each separately entitled paid DLC from
the official delivery flow associated with the CFX/Tebex account. Extract each
asset before copying it to the server.

Do not decrypt or edit protected Asset Escrow files. Customer configuration,
server-only integration settings, persistence data, and documentation are the
supported customization surfaces.

## 2. Verify the folder structure

Each resource folder must retain its exact name and contain `fxmanifest.lua` at its
root:

```text
resources/
`-- [sonoran]/
    |-- eventsCore/
    |   `-- fxmanifest.lua
    |-- eventsCctv/
    |   `-- fxmanifest.lua
    `-- eventsDashcam/
        `-- fxmanifest.lua
```

Do not use an extra nested directory such as:

```text
resources/eventsCore/eventsCore/fxmanifest.lua
```

Do not rename `eventsCore`, `eventsCctv`, or `eventsDashcam`. The names are part of
the dependency and provider contracts.

## 3. Configure OneSync and start order

Start the core before either DLC:

```cfg
set onesync on
ensure eventsCore
ensure eventsCctv
ensure eventsDashcam
```

Remove an `ensure` line for a DLC that is not installed. `eventsCore` can run by
itself. A DLC that cannot verify the required core version blocks provider startup
and does not expose a partial recording service.

## 4. Configure customer files

Edit only the supported configuration surfaces:

* `eventsCore/config/config.lua`, `storage.lua`, `permissions.lua`, and
  `integrations.lua`.
* `eventsCctv/config/` files for camera, terminal, persistence, remote access,
  permissions, and update settings.
* `eventsDashcam/config/` files for product branding, eligibility, permissions,
  triggers, retention, integrations, and update settings.

Keep API tokens, Discord webhooks, and private endpoints in server-only files. Do
not put credentials in shared configuration, NUI files, or a client integration.

## 5. First boot check

After restarting the server, verify:

1. `eventsCore` starts before the installed DLCs.
2. Each resource reports version `1.0.0` in its startup/version-check output.
3. No dependency or invalid-configuration error blocks provider registration.
4. Your ACE groups can open the intended product interface.
5. A test session can be created and reviewed before production use.

The automated repository suite does not prove GTA native behavior, OneSync routing,
Asset Escrow entitlement, or external-provider connectivity. Complete the server
smoke matrix with the licensed target environment.
