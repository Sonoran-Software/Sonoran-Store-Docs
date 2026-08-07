---
title: Dashcam permissions
published: true
date: 2026-08-06T00:00:00.000Z
tags: [fivem, events dashcam, permissions, ace]
editor: markdown
dateCreated: 2026-08-06T00:00:00.000Z
description: Dashcam-specific permission tiers and trusted integration callers.
---

# Dashcam permissions

The suite [Permissions](../permissions.md) page lists all nodes. A practical
least-privilege setup is:

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

The one-argument server export `GetDashcamRecording(recordingId)` is restricted to
server resources listed in `Config.Permissions.TrustedResources`. Player-scoped
calls must supply a source and are reauthorized. Keep the trusted list empty until
an integration has been reviewed.
