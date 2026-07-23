# Installation

This guide covers downloading, installing, and locating the Sonoran Scarf EUP
item for male and female FiveM characters.

{% hint style="info" %}
The resource folder must be named `sonoran-scarf` and started with
`ensure sonoran-scarf`.
{% endhint %}

{% hint style="info" %}
This EUP asset is escrow protected by Tebex. [Learn more about what this means.](installation.md#texture-customization)
{% endhint %}

## Before you begin

You will need:

* Access to the Cfx.re Portal account that owns the Scarf package.
* Access to the FiveM server's `resources` directory.
* Access to the server's `server.cfg`.
* A way to fully restart the server after installation.

## Download the resource

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Open the purchased assets or packages area for the account that owns the
   Scarf.
3. Download the Scarf ZIP file.

{% hint style="warning" %}
The downloaded archive can contain another `sonoran-scarf.zip`. Continue
extracting until you can see the `sonoran-scarf` resource folder and its
`fxmanifest.lua`.
{% endhint %}

## Install the resource

1. Extract the downloaded ZIP.
2. If it contains another `sonoran-scarf.zip`, extract that archive too.
3. Locate the complete `sonoran-scarf` resource folder.
4. Copy it directly into the server's `resources` directory:

   ```text
   server-data/
   └── resources/
       └── sonoran-scarf/
           ├── fxmanifest.lua
           ├── mp_f_freemode_01_sonoran_scarf.meta
           ├── mp_m_freemode_01_sonoran_scarf.meta
           └── stream/
   ```

   Do not create an extra nested folder such as
   `sonoran-scarf/sonoran-scarf/`.

5. Open `server.cfg` and add:

   ```cfg
   ensure sonoran-scarf
   ```

6. Save the file and fully restart the FiveM server.
7. Check the startup console for missing resource, manifest, or streamed-file
   errors.

## Locate the Scarf in-game

The Scarf supports both male and female freemode characters.

1. Join the server with a male or female freemode character.
2. Open **vMenu**.
3. Open **Player Appearance**.
4. Open **Neck and Scarves** or the equivalent accessories category.
5. Scroll to the end of the category and select the Sonoran Scarf.
6. Use the texture/variation controls to preview all 26 textures.

{% hint style="info" %}
The exact category label and item number can vary with the vMenu version and
other installed EUP resources. The Scarf appears at the end of the
neck/accessory clothing component.
{% endhint %}

## EUP streamed-asset troubleshooting

If the resource starts but the Scarf is missing, invisible, or causes a client
error when selected, check the following.

### 1. Confirm the resource installation

* Confirm the folder is named `sonoran-scarf`.
* Confirm `fxmanifest.lua` is directly inside that folder.
* Confirm `server.cfg` contains `ensure sonoran-scarf`.
* Confirm the server was fully restarted.
* Review the startup console for resource or streamed-file errors.

### 2. Check the Cfx.re Element Club subscription

Some streamed EUP assets require an active Cfx.re **Element Club** subscription.
Verify that the server owner's subscription is active and associated with the
account used for the server's Cfx.re license/key.

{% hint style="warning" %}
Never share license keys, account information, or payment details in support
screenshots.
{% endhint %}

### 3. Temporarily set the maximum players to 10

As a streamed-asset troubleshooting step, temporarily add or update:

```cfg
sv_maxclients 10
```

Fully restart the server and test the Scarf again. After the asset works,
restore the intended player count and verify that the server's Cfx.re plan
supports it.

### 4. Clear the client cache and reconnect

After changing streamed assets or server settings:

1. Close FiveM.
2. Clear the FiveM client cache using the normal cache-cleaning procedure.
3. Restart FiveM and reconnect.
4. Check the neck/accessory clothing category again.

### 5. Test for EUP resource conflicts

Temporarily disable other recently added EUP or clothing resources and restart
the server. If the Scarf appears, re-enable those resources one at a time to
identify the conflict.

### 6. Review client and server logs

Look for errors mentioning:

* `sonoran-scarf`
* Streamed assets
* EUP clothing components
* Asset or archetype limits
* Texture or drawable loading

When requesting support, include the relevant error text, the FiveM build, the
vMenu version, and a list of other EUP resources. Redact private information.

## Quick verification checklist

* [ ] The resource folder is named `sonoran-scarf`.
* [ ] `fxmanifest.lua` is directly inside the resource folder.
* [ ] The resource is directly inside the server's `resources` directory.
* [ ] `server.cfg` contains `ensure sonoran-scarf`.
* [ ] The server was fully restarted without resource errors.
* [ ] The Scarf is visible for both male and female freemode characters.
* [ ] All 26 textures can be previewed.
* [ ] If the asset does not stream, Element Club status and streamed-asset
      limits were checked.

## Escrow protection

The Sonoran Scarf EUP is protected through Tebex and Cfx.re Asset Escrow. This
helps prevent unauthorized redistribution, leaks, and resale.

### Runs as a separate resource

Because the asset is escrow protected, it must run as the separate
`sonoran-scarf` resource and cannot be merged into another clothing pack.

### Texture customization

The escrow-protected model cannot be edited through Durty Cloth Tool. The
included texture dictionary (`.ytd`) files can still be customized with OpenIV.
