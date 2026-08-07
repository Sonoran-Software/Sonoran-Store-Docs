---
title: Dashcam vehicle setup
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events dashcam, vehicles, eligibility]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Configure Dashcam vehicle models, jobs, plates, inventory, and server binding checks.
---

# Dashcam vehicle setup

## Shipped eligibility

The default policy accepts:

* emergency vehicle class when the server-observed vehicle also passes the other
  checks;
* model `police`;
* jobs `police`, `sheriff`, `ambulance`, and `fire`;
* inventory item `dashcam` when an inventory adapter is configured.

Configured plate and fleet-model lists are empty by default. Add local fleet models
or plate prefixes only after testing server-side binding and permissions.

## Server authority

The server re-checks the current player, driver, model, plate, job, inventory,
routing bucket, networked entity, and vehicle binding for each request. Client
vehicle class 18 is advisory; it cannot authorize a mismatched or stale vehicle.
Network IDs are accepted only for networked entities with a positive ID.

## Framework context

Standalone, ESX, QBCore, and Qbox context can be used when configured. The default
permission source remains ACE. Job grades ship at `0` for police, sheriff,
ambulance, and fire, with elevated grade `2` for administrative workflows.

Do not run a second standalone Dashcam capture engine for the same vehicle. The
Events DLC registers one logical moving camera and routes recording work through
`eventsCore`.
