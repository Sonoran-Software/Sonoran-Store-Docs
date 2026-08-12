---
title: Vehicle Setup
published: true
date: 2026-08-12T00:00:00.000Z
tags: [fivem, dashcam, vehicles, eligibility]
editor: markdown
dateCreated: 2026-08-12T00:00:00.000Z
description: Choose which emergency, department, fleet, and inventory-equipped vehicles can use Dashcam.
---

# Dashcam vehicle setup

A player must be the driver of an eligible, networked vehicle before a recording can start. Vehicle eligibility and recording permissions are separate: the vehicle must pass this policy, and the driver must also have dashcam.record through ACE or a configured framework job grade.

## Shipped eligibility

The default vehicle policy accepts a vehicle when at least one configured condition matches:

* The vehicle is in FiveM emergency class 18 and allowEmergencyClass is enabled
* Its model appears in models; police is included by default
* Its plate appears in plates
* Its model appears in civilianFleetModels
* Its plate appears in fleetPlates
* The driver's framework job appears in jobs; police, sheriff, ambulance, and fire are included by default
* A configured inventory bridge confirms the driver has the dashcam item

The plate and civilian-fleet lists are empty by default. The inventory item name is configured, but the shipped inventory bridge does not grant eligibility until it is connected to your inventory system.

## Configure your fleet

Edit eventsDashcam/config/vehicles.lua. The shipped structure is:

~~~lua
Config.Vehicles = {
    allowEmergencyClass = true,
    models = { 'police' },
    plates = {},
    jobs = {
        police = true,
        sheriff = true,
        ambulance = true,
        fire = true
    },
    civilianFleetModels = {},
    fleetPlates = {},
    inventoryItem = 'dashcam'
}
~~~

Use vehicle spawn names in the model lists. Plate matching ignores leading and trailing spaces and is not case-sensitive. Keep plate entries specific enough that they do not unintentionally authorize unrelated vehicles.

## Example custom fleet

~~~lua
Config.Vehicles.allowEmergencyClass = false
Config.Vehicles.models = { 'police', 'police2', 'ambulance' }
Config.Vehicles.plates = { 'LSPD101', 'EMS12' }
Config.Vehicles.jobs = {
    police = true,
    sheriff = true,
    ambulance = true
}
Config.Vehicles.civilianFleetModels = { 'speedo' }
Config.Vehicles.fleetPlates = { 'MOBILE1' }
~~~

This example requires an explicit model, plate, fleet entry, or configured job instead of accepting every emergency-class vehicle.

## Framework jobs

Job eligibility requires Events Core to be configured for ESX, QBCore, Qbox, or supported automatic detection. The player must be on duty when the framework reports duty state. Job action grades are configured separately in eventsDashcam/config/permissions.lua.

## Test the policy

After restarting both resources:

1. Enter an allowed vehicle as the driver and start a recording.
2. Enter the same vehicle as a passenger and confirm recording is denied.
3. Test an unlisted non-emergency vehicle and confirm it is denied.
4. Test every custom job, model, or plate entry you added.
5. Change vehicles while recording and confirm the new vehicle must pass eligibility again.

<!-- PROMO PLACEHOLDER: Add .gitbook/assets/dashcam-fleet.webp here. Recommended shot: several configured police, sheriff, fire, and EMS vehicles with the Dashcam overlay. -->
