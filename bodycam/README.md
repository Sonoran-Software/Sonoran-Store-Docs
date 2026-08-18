---
title: Bodycam
published: true
date: 2026-08-18T00:00:00.000Z
tags: [fivem, bodycam, body-worn camera, evidence, historic playback]
editor: markdown
dateCreated: 2026-08-18T00:00:00.000Z
description: Preserve incidents from an officer's body-worn perspective and manage the resulting evidence.
---

# Bodycam

Bodycam gives public-safety personnel a body-worn evidence workflow inside FiveM. It preserves activity before and after an incident, supports manual and automatic recording triggers, and lets authorized staff review the event later from the recorded wearer's perspective.

{% hint style="info" %}
**Bodycam is a paid DLC for the free Events Core package.** Install Events Core once to provide the shared recording, replay, storage, and event logic required by Bodycam and other Events products. Events Core does not add a separate gameplay interface by itself.
{% endhint %}

## Preserve the wearer's perspective

Each eligible wearer receives one server-authorized camera attached to the configured ped bone. The rolling pre-event buffer helps preserve what led up to an incident instead of starting only after the record button was pressed.

A recording can begin from:

* A manual recording or saved buffer
* The wearer's weapon discharge
* Configured wearer damage or officer-down thresholds
* A panic activation
* A configured dispatch or integration trigger

If another trigger occurs during an active recording, Bodycam adds an evidence marker to the same timeline instead of creating a duplicate recording.

## Review historic evidence

Bodycam reconstructs the recorded incident inside FiveM from the saved gameplay state. Reviewers can play, pause, seek, skip, step through the timeline, change replay speed, and return to the single recorded bodycam perspective after the original wearer and scene are gone.

The standard replay is not a conventional video file. Optional video generation, external storage, CAD, or Discord workflows require a separately configured Events Core integration.

## Control access and custody

ACE permissions and optional framework job grades control who can wear and record with a bodycam, review their own or departmental evidence, add notes, classify records, lock or archive evidence, change retention, and delete unprotected records.

Wearer eligibility can use permission/job access by itself, accept any enabled permission, inventory, or EUP check, or require every enabled check. EUP and inventory eligibility are evaluated on the server through editable integration adapters.

## How it works

1. An eligible, authorized player joins or changes into an approved duty state.
2. Bodycam binds one camera to the server-observed player ped.
3. A manual or automatic trigger starts a recording with pre-event context.
4. The finalized record appears in the Body-Worn Camera Evidence terminal.
5. An authorized reviewer searches for the record and reconstructs the incident from the recorded perspective.

## Requirements

| Requirement | Details |
| --- | --- |
| Events Core | Free required package: [download Events Core](https://sonoran.store/packages/7606652-events-core) |
| Events Bodycam | Paid Bodycam DLC from the [Sonoran Store](https://sonoran.store/) |
| FXServer | A current FiveM server with OneSync enabled |
| Permissions | ACE permissions or a supported framework job setup |
| Framework | Standalone by default; ESX, QBCore, and Qbox context can be configured |
| Database | No SQL database is required by the default configuration |

## Start here

1. Follow [Installation](installation.md) to install Events Core and Bodycam in the correct order.
2. Review [Configuration](configuration.md), [Wearer Setup](wearer-setup.md), and [Permissions](permissions.md).
3. Test manual and automatic [Recording](recording.md) with an eligible wearer.
4. Open the finalized record and test [Playback and Evidence](playback.md).

Building an integration? See the [Developer API](developer-api.md) for supported exports and lifecycle events.

For help, see [Troubleshooting](troubleshooting.md) or contact [Sonoran Software support](https://support.sonoransoftware.com/).
