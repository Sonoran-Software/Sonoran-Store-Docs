---
title: Installation
published: true
date: 2026-08-12T00:00:00.000Z
tags: [fivem, dashcam, installation, events core]
editor: markdown
dateCreated: 2026-08-12T00:00:00.000Z
description: Install the free Events Core dependency and paid Dashcam DLC in the correct order.
---

# Dashcam installation

Dashcam uses two separate resources:

* [Events Core](https://sonoran.store/packages/7606652-events-core) is the free required package containing the shared recording, replay, storage, and event logic.
* [Events Dashcam](https://sonoran.store/packages/7618996-events-dashcam) is the paid DLC containing vehicle eligibility, recording controls, the Mobile Evidence terminal, and the six dashcam replay views.

{% hint style="warning" %}
Both packages are required. Events Core works behind the scenes and does not add a standalone gameplay interface.
{% endhint %}

## Before you begin

You need:

* A current FXServer with OneSync enabled
* Access to the Cfx.re account that owns the granted assets
* Access to the server's resource directory and server.cfg
* Permission to add ACE rules or configure supported framework jobs
* A test server or maintenance window for the first installation

Both resources are protected by FiveM Asset Escrow. Edit only the included customer configuration and integration files.

## Download the resources

1. Obtain the free Events Core package from the Sonoran Store.
2. Obtain the paid [Dashcam package](https://sonoran.store/packages/7618996-events-dashcam) from the Sonoran Store.
3. Sign in to the [Cfx.re Portal](https://portal.cfx.re/) using the account associated with the purchase.
4. Download and extract both granted assets.

## Verify the folders

Keep the exact resource names. Each fxmanifest.lua must be directly inside its resource folder:

~~~text
resources/
└── [sonoran]/
    ├── eventsCore/
    │   └── fxmanifest.lua
    └── eventsDashcam/
        └── fxmanifest.lua
~~~

Do not add an extra nested folder such as eventsDashcam/eventsDashcam/fxmanifest.lua, and do not rename either resource.

## Configure the start order

Add the following to server.cfg:

~~~cfg
set onesync on
ensure eventsCore
ensure eventsDashcam
~~~

Events Core must start first. Dashcam will block startup if Core is missing, outdated, or unavailable.

## Grant initial permissions

For a first administrator test:

~~~cfg
add_ace group.dashcamadmin dashcam.use allow
add_ace group.dashcamadmin dashcam.record allow
add_ace group.dashcamadmin dashcam.review.all allow
add_ace group.dashcamadmin dashcam.annotate allow
add_ace group.dashcamadmin dashcam.classify allow
add_ace group.dashcamadmin dashcam.archive allow
add_ace group.dashcamadmin dashcam.lock allow
add_ace group.dashcamadmin dashcam.delete allow
add_ace group.dashcamadmin dashcam.admin allow
~~~

Assign the appropriate player identifier to group.dashcamadmin using your server's normal ACE setup. See [Permissions](permissions.md) before granting production access.

## Configure an eligible vehicle

The shipped setup recognizes emergency-class vehicles, the model police, and the configured police, sheriff, ambulance, and fire jobs. Review [Vehicle Setup](vehicle-setup.md) before testing a custom fleet.

## First test

1. Fully restart the server and confirm eventsCore starts before eventsDashcam.
2. Join with the test account and enter an eligible vehicle as the driver.
3. Press F7 to open the Mobile Evidence terminal.
4. Press F9 to start a manual recording.
5. Press F10 to add an evidence marker.
6. Press F9 again to finalize the recording.
7. Reopen the terminal, select the record, and test all six replay views.
8. Repeat with an unauthorized account or ineligible vehicle and confirm recording is denied.

## Updating

1. Stop the server and back up eventsCore/config/, eventsCore/data/, eventsDashcam/config/, and any customized eventsDashcam/integrations/ files.
2. Download the latest granted packages.
3. Replace the complete resource folders.
4. Merge your customer settings into the new configuration structure instead of copying protected files from an older version.
5. Start Events Core before Dashcam and repeat the first test.

Do not run another Dashcam recording resource for the same vehicle while Events Dashcam is active. Backups may contain player, vehicle, or incident information; store them securely and redact private information before sending anything to support.
