---
title: Administrative Webhooks
published: true
date: 2026-07-28T00:00:00.000Z
tags: [discord, webhooks, auditing, security]
editor: markdown
dateCreated: 2026-07-28T00:00:00.000Z
description: Configure secure Discord-compatible auditing for Sonoran Station Displays administrative actions.
---

# Administrative Webhooks

Administrative webhooks provide a non-blocking external audit trail. They supplement
server permissions, validation, rate limiting, and persistence; they are not a
persistence or security mechanism.

## Setup

Create a webhook under the Discord channel's **Integrations > Webhooks** settings, then
edit the server-only
`sonoran-stationdisplay/server/admin_webhook_config.lua` file:

```lua
Config.AdminWebhook = {
    enabled = true,
    url = "https://discord.com/api/webhooks/WEBHOOK_ID/WEBHOOK_TOKEN",
    username = "Sonoran CAD Displays",
    includeIdentifiers = true
}
```

This is a placeholder example. The shipped URL is blank. Restart the resource after
changing it. Blank, unresolved-placeholder, non-HTTPS, local, and malformed endpoints
remain disabled quietly. Discord-compatible endpoints must use the secure
`/api/webhooks/ID/TOKEN` shape.

The URL is never loaded by a client. Internal colors, HTTP timeout, retry behavior,
queue and field limits, duplicate suppression, supported schemes, event names, and
sanitization rules are protected.

## What is logged

Successful entries cover display creation, meaningful edits, movement, deletion,
service filters, page enable/order/default changes, rotation and map settings, force
refresh, full resynchronization, supported server-export page/filter/refresh actions,
the explicit server integration diagnostic, and webhook tests.

Current action titles are:

```text
DISPLAY_CREATED
DISPLAY_UPDATED
DISPLAY_MOVED
SERVICE_FILTER_CHANGED
DISPLAY_PAGES_UPDATED
BODYCAM_PAGE_CONFIG_CHANGED
DISPLAY_PAGE_FORCED
DISPLAY_DELETED
DISPLAY_FORCE_REFRESHED
FULL_DISPLAY_RESYNC
ADMIN_DIAGNOSTIC_RUN
ADMIN_WEBHOOK_TESTED
```

Rotation and map field changes are included in `DISPLAY_UPDATED`; they do not invent
separate action titles. Denied administrative requests use
`UNAUTHORIZED_ADMIN_ACTION`.

Only changed normalized values are included in edit embeds. Coordinate and rotation
tolerances prevent insignificant floating-point noise. An edit with no real change
produces a local no-change audit but no redundant webhook.

Ordinary nearby page scrolling, map panning, bodycam Page Up/Page Down navigation, and
routine feed/runtime changes are not administrative and are not logged. Duplication,
import/export, backup restore, migrations, persistence
clearing, and runtime model/map-asset editing are not implemented, so they are not
claimed as logged actions.

Security alerts are visually distinct. They cover unauthorized mutations or webhook
tests, spoofed display IDs, invalid proximity/routing context, prohibited models,
arbitrary page or DUI URL input, coordinate/render-distance limit violations, and
repeated server-event rate-limit violations. Repeated equivalent alerts are
deduplicated and escalated periodically to avoid flooding.

## Identity, privacy, and audit IDs

Player name and server ID are resolved server-side. With `includeIdentifiers = true`,
available Sonoran, Discord, license, FiveM, and Steam identifiers are included and
length-limited.

The embed also includes the server hostname. Review that value and identifier collection
before enabling a webhook in a channel with broad membership.

The webhook never includes IP addresses, API keys, Sonoran credentials, Tebex data,
authentication tokens, webhook URLs, arbitrary client embed content, or full CAD cache
payloads.

Every action has an ID such as `DSP-20260728-A1B2C3`. Use it to correlate the local
server log, webhook, administrator notification, and a related persistence error.
Webhook timestamps use real-world UTC; the mounted display clock continues to use
GTA/FiveM in-game time.

## Test the webhook

Grant:

```ini
add_ace group.admin sonoran.stationdisplay.webhook.test allow
```

Open **Mounted Displays > View Diagnostics > Test Admin Webhook**. The server validates
the private URL, rechecks permission, applies the normal event rate limit and a stricter
test cooldown, then reports delivery success or failure with an audit ID. The test does
not reveal the URL to the client.

## Delivery behavior

Messages are queued and processed in order with a 100-entry memory limit, a 750 ms
minimum send gap, and a 15-second HTTP timeout. A delivery gets at most two retries
(three total attempts). Normal retries wait 2.5 seconds; a Discord 429 uses its
`retry_after` value clamped to 1-30 seconds. Timeouts, 429, and 5xx responses retry;
other 4xx responses do not.

Permanent failures produce concise, deduplicated server warnings. Pending queue entries
exist only in memory and are discarded when the resource stops.

Webhook delivery is intentionally non-blocking. A valid administrative action can
succeed while Discord is unavailable, and the structured local audit still occurs.

## Protect the credential

* Never publish or commit a real webhook URL.
* Never share it in a support ticket without redacting the token.
* Never put it in client files, screenshots, or public examples.
* Regenerate it immediately if exposed.

## Troubleshooting

### Webhook messages are not arriving

Check that it is enabled, the URL is configured, the channel and webhook still exist,
outbound HTTPS is permitted, and the console has no response-code warning. Run the
webhook test.

### Webhook returns 401, 403, or 404

Regenerate the Discord webhook and replace the configured URL.

### Webhook returns 429

Allow the internal queue to retry and investigate excessive administrative or security
actions.

### Actions work but webhook logging fails

This is intentional non-blocking behavior. The valid action and local audit are not
rolled back by an endpoint outage.

### Duplicate webhook messages

Check for duplicate resource starts, repeated event registration, or proxy retry
behavior.

### No identifiers appear

Check `includeIdentifiers` and whether the player has the relevant identifier connected.

### Webhook test option is unavailable

Grant `sonoran.stationdisplay.webhook.test`, then reconnect or restart the resource.

The explicit test has a 60-second cooldown.
