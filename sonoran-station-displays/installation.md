---
title: Installation
published: true
date: 2026-07-28T00:00:00.000Z
tags: [fivem, installation, tebex]
editor: markdown
dateCreated: 2026-07-27T00:00:00.000Z
description: Install Sonoran Station Displays, SonoranCADFiveM, permissions, and a first test display.
---

# Installation

## Before you begin

You need:

* A current FXServer with OneSync
* SonoranCADFiveM version `4.0.72` or newer for units and calls
* Access to the CFX account that owns the Tebex asset
* Permission to add resources and ACE entries to the server

The resource does not enforce a specific FXServer artifact number. Use a supported current artifact. No framework, SQL database, menu library, Node.js runtime, or `radio_fivem` installation is required.

## 1. Download the resource

After purchasing the resource, download it from the CFX portal account that owns the asset. See [Accessing Tebex Assets](../general/tebex-assets.md) for the established store download process.

Extract the download before copying it to the server.

## 2. Verify the folder structure

The folder containing `fxmanifest.lua` must be named:

```text
sonoran-stationdisplay
```

A correct installation looks like:

```text
resources/
└── [sonoran]/
    └── sonoran-stationdisplay/
        ├── fxmanifest.lua
        ├── config.lua
        ├── client/
        ├── server/
        ├── data/
        └── web/
```

Do not leave an extra nested folder:

```text
resources/sonoran-stationdisplay/sonoran-stationdisplay/fxmanifest.lua
```

If that occurs, move the inner `sonoran-stationdisplay` folder into the resources directory and remove the empty outer folder.

## 3. Install or update SonoranCADFiveM

Install SonoranCADFiveM according to the [SonoranCADFiveM installation guide](https://info.sonorancad.com/integration-plugins/in-game-integration/fivem-installation).

The default dependency resource name is:

```text
sonorancad
```

Sonoran Station Displays requires version `4.0.72` or newer and the local `GetUnitCache`, `GetCallCache`, and `GetEmergencyCache` exports. It does not require a second CAD API key.

The optional Bodycams page additionally needs a package/build that exposes `GetBodycamRuntime()` and the runtime event in `PEER_STREAM` mode. The base `4.0.72` cache requirement does not guarantee that every package with that version contains the newer bodycam bridge.

The dependency name is managed internally and must remain `sonorancad`.

## 4. Configure start order

Add:

```ini
ensure sonorancad
ensure sonoran-stationdisplay
```

Start SonoranCADFiveM first. Sonoran Station Displays can remain started if CAD stops; displays change to a disconnected state and retry automatically when CAD returns.

The current preconfigured SonoranCADFiveM bundle may instruct you to use `exec @sonorancad/sonorancad.cfg` instead of manually ensuring its resources. In that installation, place the `exec` line before `ensure sonoran-stationdisplay`; the required core resource must still start as `sonorancad`.

Do not add `ensure radio_fivem` for this product unless your server separately uses Sonoran Radio. The radio resource is not a dependency.

## 5. Add ACE permissions

For an administrator group:

```ini
add_ace group.admin sonoran.display.view allow
add_ace group.admin sonoran.display.place allow
add_ace group.admin sonoran.display.edit allow
add_ace group.admin sonoran.display.delete allow
add_ace group.admin sonoran.display.diagnostics allow
add_ace group.admin sonoran.display.webhook.test allow
```

Grant `sonoran.display.view` to each group that should receive display configuration and CAD cache data. See [Permissions](permissions.md) for least-privilege examples and custom permission mode.

## 6. Review configuration

Open `sonoran-stationdisplay/config.lua` and confirm:

* `Config.PermissionMode = "ace"`
* Default pages, service filter, theme, rotation, clock/weather, and distances
* Live Map and bodycam defaults
* The included display model registry

Update checks, the dependency name/minimum version, DUI lifecycle, and security rate
limits require no customer configuration. Do not place API keys, Tebex credentials, or
license credentials in this file, and do not modify protected implementation files.

## 7. Restart and verify startup

Restart the server. In the server console, expect messages similar to:

```text
[Sonoran Station Displays] Connected to SonoranCADFiveM 4.0.72 using supported cache exports.
[Sonoran Station Displays] Loaded 0 configured display(s).
```

On first start, the resource creates:

```text
sonoran-stationdisplay/data/displays.json
```

If CAD is missing, old, or incompatible, the server prints an `SSD-CAD-*` error. See [Troubleshooting](troubleshooting.md).

## 8. Place a test display

1. Join with the administrator ACE permissions.
2. Confirm at least one unit is on duty in Sonoran CAD.
3. Run `/stationdisplay`.
4. Select **Mounted Displays**.
5. Select **Place New Display**.
6. Choose a model, service filter, and one or more pages.
7. Leave the rotation interval at the default 30 seconds.
8. Select **Continue to Placement**.
9. Position the preview within 15 meters, then select **Confirm Placement**.
10. Walk within the display's render distance and confirm current state appears without the CAD unavailable overlay.

If the display has no rows, confirm its service filter matches the on-duty unit's `unit.data.page` classification.

## Tebex Asset Escrow

The production package is intended for FiveM Asset Escrow. Customer-editable files are excluded from encryption:

* `config.lua`
* `server/admin_webhook_config.lua`
* `locales/*.lua`
* `data/displays.DEFAULT.json`
* `docs/*.md`
* `README.md`

Runtime display data is written to `data/displays.json`. Back it up before replacing the resource during an update.

## Source builds

Customers do not build the web interface. It ships as static production files and has no Node.js dependency.

Repository maintainers can validate and package a source checkout from the repository root:

```powershell
python scripts/validate.py
python scripts/package.py
```

The package command creates `build/stationDisplay.zip` with one top-level `sonoran-stationdisplay` folder.
