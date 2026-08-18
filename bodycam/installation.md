---
title: Installation
published: true
date: 2026-08-18T00:00:00.000Z
tags: [fivem, bodycam, installation, events core]
editor: markdown
dateCreated: 2026-08-18T00:00:00.000Z
description: Install the free Events Core dependency and paid Bodycam DLC in the correct order.
---

# Bodycam installation

Bodycam uses two separate resources:

* [Events Core](https://sonoran.store/packages/7606652-events-core) is the free required package containing the shared recording, replay, storage, and event logic.
* Events Bodycam is the paid DLC containing wearer eligibility, recording controls, the evidence terminal, and the body-worn replay perspective.

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
2. Obtain the paid Bodycam package from the Sonoran Store.
3. Sign in to the [Cfx.re Portal](https://portal.cfx.re/) using the account associated with the purchase.
4. Download and extract both granted assets.

## Verify the folders

Keep the exact resource names. Each fxmanifest.lua must be directly inside its resource folder:

~~~text
resources/
└── [sonoran]/
    ├── eventsCore/
    │   └── fxmanifest.lua
    └── eventsBodycam/
        └── fxmanifest.lua
~~~

Do not add an extra nested folder such as eventsBodycam/eventsBodycam/fxmanifest.lua, and do not rename either resource.

## Configure the start order

Add the following to server.cfg:

~~~cfg
set onesync on
ensure eventsCore
ensure eventsBodycam
~~~

Events Core must start first. Bodycam blocks startup if Core is missing, incompatible, or unavailable.

## Grant initial permissions

For a first administrator test:

~~~cfg
add_ace group.bodycamadmin bodycam.use allow
add_ace group.bodycamadmin bodycam.record allow
add_ace group.bodycamadmin bodycam.review.all allow
add_ace group.bodycamadmin bodycam.annotate allow
add_ace group.bodycamadmin bodycam.classify allow
add_ace group.bodycamadmin bodycam.archive allow
add_ace group.bodycamadmin bodycam.lock allow
add_ace group.bodycamadmin bodycam.delete allow
add_ace group.bodycamadmin bodycam.admin allow
~~~

Assign the appropriate player identifier to group.bodycamadmin using your server's normal ACE setup. See [Permissions](permissions.md) before granting production access.

## Configure an eligible wearer

The shipped `permission` eligibility mode accepts a wearer with bodycam.use through ACE or a configured on-duty framework job. Inventory and EUP checks are disabled by default. Review [Wearer Setup](wearer-setup.md) before enabling either adapter.

## First test

1. Fully restart the server and confirm eventsCore starts before eventsBodycam.
2. Join with the authorized test account.
3. Press F6 or run /bodycam to open the evidence terminal.
4. Press F11 to start a manual recording.
5. Run /bodycammark Initial test to add an evidence marker.
6. Press F11 again to finalize the recording.
7. Reopen the terminal, select the record, and open structured replay.
8. Repeat with an unauthorized account and confirm recording is denied.

## Updating

1. Stop the server and back up eventsCore/config/, eventsCore/data/, eventsBodycam/config/, and any customized eventsBodycam/integrations/ files.
2. Download the latest granted packages.
3. Replace the complete resource folders.
4. Merge your customer settings into the new configuration structure instead of copying protected files from an older version.
5. Start Events Core before Bodycam and repeat the first test.

Do not run another bodycam recording resource for the same wearer while Events Bodycam is active. Backups may contain player, location, and incident information; store them securely and redact private information before sending anything to support.
