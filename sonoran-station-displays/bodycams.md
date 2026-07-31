---
title: Bodycams
published: true
date: 2026-07-28T00:00:00.000Z
tags: [bodycams, screenshot-basic, local capture]
editor: markdown
dateCreated: 2026-07-28T00:00:00.000Z
description: Configure, secure, and troubleshoot periodic local bodycam imagery on mounted displays.
---

# Bodycams

The `BODYCAMS` page displays periodic live images captured locally from active,
on-duty officers. It is not continuous video. Station Displays no longer uses the
previous PeerJS/TURN viewer, remote image relay, or hosted bodycam URL.

<figure><img src="../.gitbook/assets/station-displays-bodycams-grid.webp" alt="Station Displays bodycam grid showing controlled connection and off states"><figcaption><p>The production four-tile renderer. This controlled capture contains no officer footage.</p></figcaption></figure>

## Requirements

* Start the core resource as `sonorancad`.
* Install and start `screenshot-basic`.
* Use a SonoranCADFiveM build with `GetBodycamRuntime()` and
  `SonoranCAD::bodycam::RuntimeChanged`.
* Enable the Sonoran bodycam submodule, put the officer on duty, and activate it.
* No viewing ACE is required; image delivery is limited to eligible nearby viewers.

Recommended resource order:

```cfg
ensure sonorancad
ensure screenshot-basic
ensure sonoran-stationdisplay
```

If `screenshot-basic` is missing, the rest of Station Displays continues to operate
and the bodycam page reports an unavailable capture dependency.

## How local capture works

SonoranCADFiveM supplies authoritative bodycam active state, player source, and CAD
unit ownership. A nearby DUI subscribes only to the feeds visible on its mounted
display. The Station Displays server validates distance, routing bucket, interior,
current page, service filter, on-duty ownership, and active state.

For each active subscribed officer, the server requests one client-local capture:

```lua
exports["screenshot-basic"]:requestClientScreenshot(source, {
    encoding = "jpg",
    quality = 0.55
}, callback)
```

`screenshot-basic` returns the image through its built-in FXServer HTTP upload handler.
Station Displays validates the JPEG and keeps only the latest frame. It uses a
rate-controlled latent event to deliver that frame only to eligible subscribed
viewers. The DUI replaces and revokes its previous Blob object URL.

Several displays showing the same officer reuse the same capture. Display count never
increases the officer's capture frequency.

## Limits and expected bandwidth

* Capture interval: 4 seconds (15 frames per minute maximum).
* JPEG quality: 0.55.
* Maximum decoded frame: 300 KB.
* Maximum simultaneous visible feeds per viewing client: 4.
* Subscription heartbeat/expiry: 4/8 seconds.
* Stale frame timeout: 30 seconds.

The installed `screenshot-basic` API supports encoding and quality but not resize
options, so capture uses the officer's native game-frame dimensions and the DUI scales
it into the tile. Frames over 300 KB are rejected. At the hard limit, one feed is
about 75 KB/s (0.6 Mbps), and four feeds are about 300 KB/s (2.4 Mbps) per viewer,
before protocol overhead. Measure average and maximum JPEG sizes on the graphics
profiles used by your community.

## Layouts

| Layout | Behavior |
| --- | --- |
| `AUTO` | One feed uses single; multiple feeds use grid. |
| `SINGLE` | One large feed per bodycam page. |
| `GRID` | Up to four feeds in a 2-by-1 or 2-by-2 grid. |
| `FOCUSED` | One large feed plus a narrow secondary rail. |

Extra feeds create additional bodycam pages. These rotate independently using the
saved bodycam interval (15 seconds by default). Page Down/Page Up moves between feed
pages on the targeted DUI.

## Overlay and states

The Station Displays overlay can show recording state, callsign, unit name, agency,
status, assigned call, and capture freshness without depending on a baked-in label.

| State | Meaning |
| --- | --- |
| `CONNECTING` | Active and subscribed, but no accepted frame yet. |
| `LIVE` | The latest accepted frame is current. |
| `DELAYED` | Capture is late; the previous frame remains visible briefly. |
| `CAPTURE FAILED` | Capture, validation, or decode failed. |
| `OFFLINE` | Inactive, off duty, disconnected, or past the stale timeout. |

## Security and privacy

The server chooses the source officer, capture interval, JPEG settings, and recipients.
It does not trust client-submitted callsigns, agency labels, active state, viewers, or
frame sequence. MIME, base64 form, ownership, size, freshness, and subscriptions are
validated server-side.

Frames are never broadcast globally, written to `data/displays.json`, included in
webhooks, logged, or returned by diagnostics. Diagnostics expose sizes, timing,
sequence counters, source IDs, and subscriber counts only.

Bodycam imagery can contain sensitive roleplay information. Use intentional display
placement, interiors, routing buckets, and short render distances. Avoid placing a
bodycam page on a publicly accessible screen.

## Troubleshooting

### No bodycams appear

Check the bodycam submodule, duty/active state, service filter, enabled `BODYCAMS`
page, `GetBodycamRuntime()` bridge, and viewer range.

### Capture dependency unavailable

Start `screenshot-basic` before Station Displays and check the server console for
`SSD-BODYCAM-001`.

### Capture failed or oversized

Use diagnostics to inspect last attempt/success, upload duration, frame size, sequence,
subscriber count, and malformed/oversized counters. Lower the officer's game
resolution if quality-0.55 JPEGs repeatedly exceed 300 KB.

### Delayed or stale

The previous frame remains briefly. Confirm the officer is initialized and not in a
loading screen, pause menu, or screen fade. The frame is removed after 30 seconds.

### A viewer receives no image

Confirm render range, routing bucket, interior, the mounted display's current page,
service filter, and visible feed page.
