# Sonoran Short Slip Dress: Installation & In-Game Location

This guide covers downloading, installing, and locating the Sonoran Short Slip Dress EUP item.

{% hint style="info" %}
The resource folder should be named `sonoran-slipdress` and started with `ensure sonoran-slipdress`. If the downloaded ZIP uses a different folder name, use that exact name instead.
{% endhint %}

## Before You Begin

You will need:

* Access to the Cfx.re Portal account that purchased the Short Slip Dress.
* Access to your FiveM server's `resources` folder.
* Access to your server's `server.cfg` file.
* A way to fully restart the server after installation.

> **Screenshot placeholder — Cfx.re Portal:** Show the purchased Short Slip Dress asset and download button.

## Download the Resource

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Open the purchased assets or packages area for the account that owns the Short Slip Dress.
3. Download the Short Slip Dress `.zip` file.

> **Screenshot placeholder — Download:** Show the Short Slip Dress ZIP download in the Cfx.re Portal.

> **Screenshot placeholder — Downloaded ZIP:** Show the downloaded ZIP file on the computer.

## Install the Resource

1. Extract or unzip the downloaded file.

   > **Screenshot placeholder — Extract ZIP:** Show the extraction menu or window.

2. Locate the extracted resource folder. It should be named:

   ```text
   sonoran-slipdress
   ```

   If the package uses a different folder name, keep that exact name and use it in the `ensure` line below.

   > **Screenshot placeholder — Extracted resource:** Show the extracted resource folder.

3. Copy the complete resource folder directly into your server's `resources` directory:

   ```text
   server-data/
   └── resources/
       └── sonoran-slipdress/
   ```

   Do not create an extra nested folder such as `sonoran-slipdress/sonoran-slipdress/`. The resource manifest should be directly inside the resource folder.

   > **Screenshot placeholder — Resources folder:** Show the dress resource alongside the server's other resources.

   > **Screenshot placeholder — Correct nesting:** Show the resource manifest inside `resources/sonoran-slipdress/`.

4. Open `server.cfg` and add:

   ```cfg
   ensure sonoran-slipdress
   ```

   If your ZIP uses a different resource folder name, replace `sonoran-slipdress` with that name.

   > **Screenshot placeholder — server.cfg:** Show the `ensure sonoran-slipdress` line.

5. Save the file and fully restart the FiveM server.

   > **Screenshot placeholder — Server restart:** Show the server being restarted in txAdmin, a hosting panel, or a terminal.

6. Check the startup console for resource errors.

   > **Screenshot placeholder — Startup console:** Show the Short Slip Dress resource loading without errors.

## Locate the Short Slip Dress In-Game

The Short Slip Dress is accessed as female EUP clothing through vMenu.

1. Join the server with a female character that can access vMenu's clothing options.
2. Open **vMenu**.
3. Open **Player Appearance**.
4. Select **Shirt overlay / Jackets**.
5. Scroll all the way to the end of the category.
6. Select item **633**.
7. Use the texture/variation controls to preview the 22 available textures.

> **Screenshot placeholder — Open vMenu:** Show the vMenu clothing or Player Appearance menu.

> **Screenshot placeholder — Player Appearance:** Show **Player Appearance** selected.

> **Screenshot placeholder — Shirt overlay / Jackets:** Show the clothing category selected.

> **Screenshot placeholder — Item 633:** Show item **633** at the end of the category.

> **Screenshot placeholder — Equipped dress:** Show the Short Slip Dress equipped on a female character.

## EUP Streamed-Asset Troubleshooting

If the resource starts but the Short Slip Dress does not appear, appears invisible, or causes players to disconnect or crash when selected, the issue may be related to FiveM streamed EUP assets or server limits.

### 1. Confirm the resource installation

* Confirm the resource folder name matches the name used in `server.cfg`.
* Confirm the resource is directly inside the server's `resources` folder.
* Confirm `server.cfg` contains the correct `ensure` line.
* Confirm the server was fully restarted after installation.
* Confirm the startup console does not report a missing manifest or failed file load.

> **Screenshot placeholder — Resource structure check:** Show the correct folder name and location.

### 2. Check the Cfx.re Element Club subscription

Some EUP streamed assets require an active Cfx.re **Element Club** subscription. If the dress is installed correctly but the streamed clothing asset does not load, verify that the server owner's Element Club subscription is active and associated with the account used for the server's Cfx.re license/key.

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Check the server owner's Element Club subscription status.
3. Confirm the subscription is active for the account associated with the server license/key.
4. Restart the server after correcting the subscription or account association.

> **Screenshot placeholder — Element Club status:** Show an active Element Club subscription. Redact account IDs, license keys, payment details, and other private information.

{% hint style="warning" %}
Never post private license keys, account information, or payment details in support screenshots.
{% endhint %}

### 3. Temporarily set the maximum players to 10

If the EUP asset still does not stream, temporarily set the server's maximum player count to 10. Add or update this line in `server.cfg`:

```cfg
sv_maxclients 10
```

Fully restart the server and test the Short Slip Dress again.

> **Screenshot placeholder — Max players in server.cfg:** Show `sv_maxclients 10` in the configuration file.

> **Screenshot placeholder — txAdmin limit:** If applicable, show the matching maximum-player setting set to 10 in txAdmin or your hosting panel.

{% hint style="info" %}
Use the 10-player limit as a troubleshooting step. After the streamed asset works correctly, test a higher player count again if your server setup and Cfx.re requirements support it.
{% endhint %}

### 4. Clear client cache and reconnect

After changing streamed assets or server settings:

1. Stop FiveM.
2. Clear the FiveM client cache using your normal cache-cleaning procedure. Do not delete the entire FiveM installation.
3. Restart FiveM and reconnect to the server.
4. Open vMenu and check **Player Appearance → Shirt overlay / Jackets**, item **633**, again.

> **Screenshot placeholder — Client cache:** Show the cache-cleaning step used by your support procedure.

### 5. Test for EUP resource conflicts

Temporarily disable other recently added EUP or clothing resources and restart the server. If the dress appears, re-enable the other resources one at a time to identify the conflict.

> **Screenshot placeholder — Resource isolation:** Show the server resource list with other EUP resources disabled for a test restart.

### 6. Review client and server logs

Look for errors mentioning:

* The dress resource name
* Streamed assets
* EUP clothing components
* Asset or archetype limits
* Texture or drawable loading

When requesting support, include the relevant error text, your FiveM build, vMenu version, and a list of other EUP resources. Redact license keys and personal account information first.

> **Screenshot placeholder — Error log:** Show the relevant client or server error with private information redacted.

## Quick Verification Checklist

* [ ] The resource folder name matches the `ensure` line.
* [ ] The resource is directly inside the server's `resources` directory.
* [ ] The server was fully restarted.
* [ ] The resource has no startup errors.
* [ ] vMenu opens successfully.
* [ ] **Player Appearance → Shirt overlay / Jackets** is available.
* [ ] The Short Slip Dress is visible at item **633**, at the end of the category.
* [ ] The 22 textures can be previewed.
* [ ] If the asset does not stream, Element Club status was checked and `sv_maxclients 10` was tested.
