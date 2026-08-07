---
title: Permissions
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, ace, permissions, events]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Least-privilege ACE permissions for Events Core, CCTV, and Dashcam.
---

# Permissions

Events reauthorizes requests on the server. A client UI state, vehicle class, or
camera identifier does not grant access. Start with narrow groups and add only the
nodes required by each role.

## Core worker permission

The shipped core worker ACE is:

```cfg
add_ace group.eventsworker events.processing.worker allow
```

Only resources listed in the server-only `Config.Permissions.WorkerResources` can
register recorder, upload, or reconstruction workers. Automatic player workers are
disabled by default.

## CCTV permissions

| ACE node | Purpose |
| --- | --- |
| `cctv.admin` | Camera, terminal, persistence, and administrative operations |
| `cctv.live.view` | Live camera viewing |
| `cctv.recording.view` | Recording archive and replay access |
| `cctv.recording.create` | Manual recording/bookmark creation |
| `cctv.recording.delete` | Recording deletion |
| `cctv.recording.upload` | Upload workflow |
| `cctv.discord.send` | Discord workflow |
| `cctv.remote.all` | Remote access across terminal groups |

Example:

```cfg
add_ace group.cctvadmin cctv.admin allow
add_ace group.cctvstaff cctv.live.view allow
add_ace group.cctvstaff cctv.recording.view allow
add_ace group.cctvstaff cctv.recording.create allow
add_ace group.cctvsupervisor cctv.recording.delete allow
add_ace group.cctvsupervisor cctv.recording.upload allow
add_ace group.cctvsupervisor cctv.discord.send allow
```

Terminal-specific access uses `cctv.terminal.<terminalId>`. A terminal group uses
`cctv.remote.<group>`. `cctv.remote.all` is the broad override; prefer a group or
terminal node when possible.

## Dashcam permissions

| ACE node | Purpose |
| --- | --- |
| `dashcam.use` | Use the Dashcam product |
| `dashcam.record` | Start/stop or save a recording |
| `dashcam.review.own` | Review the caller's evidence |
| `dashcam.review.department` | Review department evidence |
| `dashcam.review.all` | Review all permitted evidence |
| `dashcam.annotate` | Add notes and markers |
| `dashcam.classify` | Change evidence classification |
| `dashcam.archive` | Archive evidence |
| `dashcam.lock` | Lock or preserve evidence |
| `dashcam.delete` | Delete evidence with a substantive reason |
| `dashcam.admin` | Administrative Dashcam operations |

Example:

```cfg
add_ace group.dashcamusers dashcam.use allow
add_ace group.dashcamusers dashcam.record allow
add_ace group.dashcamusers dashcam.review.own allow
add_ace group.dashcamsupervisors dashcam.review.department allow
add_ace group.dashcamsupervisors dashcam.annotate allow
add_ace group.dashcamsupervisors dashcam.classify allow
add_ace group.dashcamsupervisors dashcam.lock allow
add_ace group.dashcamsupervisors dashcam.archive allow
add_ace group.dashcamadmins dashcam.delete allow
```

## Framework context

The server can combine ACE with standalone, ESX, QBCore, or Qbox job context when
configured. Configure job grades and trusted resources in server-only files. Never
move tokens, webhooks, or framework secrets into shared configuration or NUI.
