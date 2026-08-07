---
title: Dashcam installation
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events dashcam, installation]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Install Events Dashcam, configure its core dependency, and verify the first eligible vehicle.
---

# Dashcam installation

Install `eventsCore` first using the suite [Installation](../installation.md)
guide. Place `eventsDashcam` as a sibling resource with `fxmanifest.lua` at its
root.

## Start order

```cfg
set onesync on
ensure eventsCore
ensure eventsDashcam
```

Only one moving vehicle camera is registered for each eligible active driver.
Restarting the core or DLC re-synchronizes current eligible vehicles; finalized
historical sessions remain core-owned.

## Configure before testing

Review:

* `eventsDashcam/config/config.lua`: branding, capture limits, triggers, keys, and
  request limits.
* `eventsDashcam/config/vehicles.lua`: models, plates, jobs, fleet, and inventory.
* `eventsDashcam/config/permissions.lua`: ACE nodes, job grades, and trusted
  server-resource callers.
* `eventsDashcam/config/retention.lua`: classification policies.
* `eventsDashcam/config/integrations.lua`: optional product hooks.

## First test

1. Grant `dashcam.use`, `dashcam.record`, and `dashcam.review.own` to a test group.
2. Enter an eligible vehicle with the configured driver/job or inventory context.
3. Open `F7`, start with `F9`, add a marker with `F10`, and stop with `F9`.
4. Review the session and open each replay view.
5. Test an ineligible vehicle and an unauthorized player; both should be denied.
