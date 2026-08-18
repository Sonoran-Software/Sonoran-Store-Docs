---
title: Permissions
published: true
date: 2026-08-18T00:00:00.000Z
tags: [fivem, bodycam, permissions, ace]
editor: markdown
dateCreated: 2026-08-18T00:00:00.000Z
description: Configure least-privilege access for Bodycam wearers, reviewers, supervisors, and administrators.
---

# Bodycam permissions

Bodycam rechecks recording and evidence actions on the server. Grant only the actions each role needs.

| ACE node | Purpose |
| --- | --- |
| bodycam.use | Qualify for permission-based wearer eligibility; action-specific permissions are still required |
| bodycam.record | Start recordings and save the current buffer as an eligible wearer |
| bodycam.review.own | List and replay the user's own recordings |
| bodycam.review.department | List and replay recordings from the user's current department |
| bodycam.review.all | List and replay all accessible Bodycam recordings |
| bodycam.annotate | Add notes and markers or update incident-related fields on accessible evidence |
| bodycam.classify | Change an accessible recording's classification |
| bodycam.archive | Archive records and change their retention |
| bodycam.lock | Lock or unlock records |
| bodycam.delete | Permanently delete unlocked and unpreserved records with a reason |
| bodycam.admin | Use protected administrative diagnostics when available |

## Example roles

~~~cfg
# Wearers can record, review their own evidence, and add markers or notes.
add_ace group.bodycamusers bodycam.use allow
add_ace group.bodycamusers bodycam.record allow
add_ace group.bodycamusers bodycam.review.own allow
add_ace group.bodycamusers bodycam.annotate allow

# Supervisors can review and manage evidence for their department.
add_ace group.bodycamsupervisors bodycam.use allow
add_ace group.bodycamsupervisors bodycam.record allow
add_ace group.bodycamsupervisors bodycam.review.department allow
add_ace group.bodycamsupervisors bodycam.annotate allow
add_ace group.bodycamsupervisors bodycam.classify allow
add_ace group.bodycamsupervisors bodycam.archive allow
add_ace group.bodycamsupervisors bodycam.lock allow

# Administrators can review all evidence and use protected actions.
add_ace group.bodycamadmins bodycam.use allow
add_ace group.bodycamadmins bodycam.record allow
add_ace group.bodycamadmins bodycam.review.all allow
add_ace group.bodycamadmins bodycam.annotate allow
add_ace group.bodycamadmins bodycam.classify allow
add_ace group.bodycamadmins bodycam.archive allow
add_ace group.bodycamadmins bodycam.lock allow
add_ace group.bodycamadmins bodycam.delete allow
add_ace group.bodycamadmins bodycam.admin allow
~~~

Assign player identifiers to these groups using your server's existing ACE structure. If supervisors or administrators also need to review their own evidence through a different role layout, grant review.own as well or configure group inheritance.

## Framework job grades

When Events Core is configured for a supported framework, on-duty jobs can authorize Bodycam actions through eventsBodycam/config/permissions.lua:

* JobGrades controls standard actions such as use, recording, own or departmental review, and annotations.
* ElevatedJobGrades controls review-all, classification, archive, lock, deletion, and administrative actions.

The shipped jobs are police, sheriff, ambulance, and fire. Their standard minimum grade is 0 and elevated minimum grade is 2. Change these values to match your framework's grade numbering and department policy.

The wearer policy can also require an inventory item or EUP component. Passing those checks does not replace action permissions such as bodycam.record unless `EligibilityMode = 'any'` is intentionally configured for base wearer eligibility; recording and evidence actions are still authorized separately.

## Trusted integrations

Leave `Config.Permissions.TrustedResources` empty unless a reviewed server integration explicitly needs the one-argument `GetBodycamRecording(recordingId)` lookup. Adding a resource grants that resource a trusted server-side evidence read path; it does not grant players additional permissions.

Player-initiated requests should always use the player-scoped lookup so Bodycam can apply that player's evidence access.

## Least-privilege guidance

* Give normal wearers review.own instead of review.all.
* Give review.department only to roles that should see coworkers' evidence.
* Restrict classify, archive, lock, and delete to supervisors or evidence custodians.
* Keep bodycam.delete separate from normal recording and review roles.
* Treat TrustedResources as a server integration allowlist, not a convenience setting.
* Test permissions with a non-administrator account before launch.
