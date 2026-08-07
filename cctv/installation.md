---
title: Installation
published: true
date: 2026-08-07T00:00:00.000Z
tags: [fivem, cctv, installation, events core]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Install the free Events Core dependency and paid CCTV DLC in the correct order.
---

# CCTV installation

CCTV uses two separate resources:

* [Events Core](https://sonoran.store/packages/7606652-events-core) is the free required package containing the shared logic used by CCTV and other Events DLC.
* [Events CCTV](https://sonoran.store/packages/7606654-events-cctv) is the paid DLC containing the camera network, archive, historic playback, and operator tools.

{% hint style="warning" %}
Both packages are required. Events Core works behind the scenes and does not add a standalone gameplay interface.
{% endhint %}

## Before you begin

You need:

* A current FXServer with OneSync enabled
* Access to the Cfx.re account that owns the granted assets
* Access to the server's resource directory and server.cfg
* Permission to add ACE rules
* A test server or maintenance window for the first installation

Both resources are protected by FiveM Asset Escrow. Edit only the included customer configuration files.

## Download the resources

1. Obtain the free Events Core package from the Sonoran Store.
2. Obtain the paid CCTV package from the Sonoran Store.
3. Sign in to the [Cfx.re Portal](https://portal.cfx.re/) using the account associated with the purchase.
4. Download and extract both granted assets.

## Verify the folders

Keep the exact resource names. Each fxmanifest.lua must be directly inside its resource folder:

~~~text
resources/
└── [sonoran]/
    ├── eventsCore/
    │   └── fxmanifest.lua
    └── eventsCctv/
        └── fxmanifest.lua
~~~

Do not add an extra nested folder such as eventsCctv/eventsCctv/fxmanifest.lua, and do not rename either resource.

## Configure the start order

Add the following to server.cfg:

~~~cfg
set onesync on
ensure eventsCore
ensure eventsCctv
~~~

Events Core must start first. CCTV will not start correctly if the required Core version is missing or outdated.

## Grant initial permissions

For a first administrator test:

~~~cfg
add_ace group.cctvadmin cctv.admin allow
add_ace group.cctvadmin cctv.live.view allow
add_ace group.cctvadmin cctv.recording.view allow
add_ace group.cctvadmin cctv.recording.create allow
~~~

Assign the appropriate player identifier to group.cctvadmin using your server's normal ACE setup. See [Permissions](permissions.md) before granting production access.

## First test

1. Fully restart the server and confirm eventsCore starts before eventsCctv.
2. Join with the test administrator account.
3. Run /cctv.
4. Create a terminal and camera in a test area.
5. Open the camera's live view.
6. Trigger motion or create a bookmark.
7. Wait for the recording to finish, then open the archive and replay it.
8. Confirm the recording can still be reviewed after the subject leaves the area.

## Updating

1. Stop the server and back up eventsCore/config/, eventsCore/data/, eventsCctv/config/, and eventsCctv/data/cctv.json.
2. Download the latest granted packages.
3. Replace the complete resource folders.
4. Merge your customer settings into the new configuration structure instead of copying protected files from an older version.
5. Start Events Core before CCTV and repeat the first test.

Backups may contain player, vehicle, or incident information. Store them securely and redact sensitive information before sending anything to support.
