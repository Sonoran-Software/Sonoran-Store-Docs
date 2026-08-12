---
title: Permissions
published: true
date: 2026-08-12T00:00:00.000Z
tags: [fivem, dashcam, permissions, ace]
editor: markdown
dateCreated: 2026-08-12T00:00:00.000Z
description: Configure least-privilege access for Dashcam drivers, reviewers, supervisors, and administrators.
---

# Dashcam permissions

Dashcam rechecks recording and evidence actions on the server. Grant only the actions each role needs.

| ACE node | Purpose |
| --- | --- |
| dashcam.use | Configured base access node; action-specific permissions are still required |
| dashcam.record | Start recordings and save the current buffer in an eligible vehicle |
| dashcam.review.own | List and replay the user's own recordings |
| dashcam.review.department | List and replay recordings from the user's department |
| dashcam.review.all | List and replay all accessible Dashcam recordings |
| dashcam.annotate | Add notes and markers or update incident-related fields on accessible evidence |
| dashcam.classify | Change an accessible recording's classification |
| dashcam.archive | Archive records and change their retention |
| dashcam.lock | Lock or unlock records |
| dashcam.delete | Permanently delete unlocked and unpreserved records with a reason |
| dashcam.admin | Use protected administrative diagnostics when available |

## Example roles

~~~cfg
# Drivers can record, review their own evidence, and add markers or notes.
add_ace group.dashcamusers dashcam.use allow
add_ace group.dashcamusers dashcam.record allow
add_ace group.dashcamusers dashcam.review.own allow
add_ace group.dashcamusers dashcam.annotate allow

# Supervisors can review and manage evidence for their department.
add_ace group.dashcamsupervisors dashcam.use allow
add_ace group.dashcamsupervisors dashcam.record allow
add_ace group.dashcamsupervisors dashcam.review.department allow
add_ace group.dashcamsupervisors dashcam.annotate allow
add_ace group.dashcamsupervisors dashcam.classify allow
add_ace group.dashcamsupervisors dashcam.archive allow
add_ace group.dashcamsupervisors dashcam.lock allow

# Administrators can review all evidence and use protected actions.
add_ace group.dashcamadmins dashcam.use allow
add_ace group.dashcamadmins dashcam.record allow
add_ace group.dashcamadmins dashcam.review.all allow
add_ace group.dashcamadmins dashcam.annotate allow
add_ace group.dashcamadmins dashcam.classify allow
add_ace group.dashcamadmins dashcam.archive allow
add_ace group.dashcamadmins dashcam.lock allow
add_ace group.dashcamadmins dashcam.delete allow
add_ace group.dashcamadmins dashcam.admin allow
~~~

Assign player identifiers to these groups using your server's existing ACE structure. If supervisors or administrators also need to review their own evidence through a different role layout, grant review.own as well or configure group inheritance.

## Framework job grades

When Events Core is configured for a supported framework, on-duty jobs can authorize Dashcam actions through eventsDashcam/config/permissions.lua:

* JobGrades controls standard actions such as recording, own or departmental review, and annotations.
* ElevatedJobGrades controls review-all, classification, archive, lock, deletion, and administrative actions.

The shipped jobs are police, sheriff, ambulance, and fire. Their standard minimum grade is 0 and elevated minimum grade is 2. Change these values to match your framework's grade numbering and department policy.

## Trusted integrations

Leave Config.Permissions.TrustedResources empty unless a reviewed integration's installation instructions explicitly require access. Adding a resource to this list grants it a trusted server-side evidence lookup path; it does not grant players additional permissions.

## Least-privilege guidance

* Give normal drivers review.own instead of review.all.
* Give review.department only to roles that should see coworkers' evidence.
* Restrict classify, archive, lock, and delete to supervisors or evidence custodians.
* Keep dashcam.delete separate from normal recording and review roles.
* Test permissions with a non-administrator account before launch.
