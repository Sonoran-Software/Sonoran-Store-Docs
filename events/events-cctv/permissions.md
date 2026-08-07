---
title: CCTV permissions
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events cctv, permissions, ace]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: CCTV-specific ACE nodes, terminal groups, and remote access policy.
---

# CCTV permissions

The suite [Permissions](../permissions.md) page lists every node. CCTV-specific
policy is:

```cfg
add_ace group.cctvadmin cctv.admin allow
add_ace group.cctvstaff cctv.live.view allow
add_ace group.cctvstaff cctv.recording.view allow
add_ace group.cctvstaff cctv.recording.create allow
add_ace group.cctvsupervisor cctv.recording.delete allow
add_ace group.cctvsupervisor cctv.recording.upload allow
add_ace group.cctvsupervisor cctv.discord.send allow
```

Use `cctv.remote.<group>` for a terminal group and `cctv.terminal.<terminalId>`
for a specific terminal. Grant `cctv.remote.all` only to operators who should
cross all terminal groups. Map-camera enrollment and cleanup require
`cctv.admin`.
