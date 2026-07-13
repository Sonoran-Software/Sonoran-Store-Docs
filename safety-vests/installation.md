# Sonoran Safety Vests: Installation & In-Game Location

This guide covers downloading, installing, and locating the Sonoran Safety Vests EUP items.

{% hint style="info" %}
The resource folder should be named `sonoran-safety-vests` and started with `ensure sonoran-safety-vests`.
{% endhint %}

## Before You Begin

You will need:

* Access to the Cfx.re Portal account that purchased the Safety Vests.
* Access to your FiveM server's `resources` folder.
* Access to your server's `server.cfg` file.
* A way to fully restart the server after installation.

> **Screenshot placeholder — Cfx.re Portal:** Show the purchased Safety Vests asset and download button.

## Download the Resource

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Open the purchased assets or packages area for the account that owns the Safety Vests.
3. Download the Safety Vests `.zip` file.

> **Screenshot placeholder — Download:** Show the Safety Vests ZIP download in the Cfx.re Portal.

> **Screenshot placeholder — Downloaded ZIP:** Show the downloaded ZIP file on the computer.

## Install the Resource

1. Extract or unzip the downloaded file.

   > **Screenshot placeholder — Extract ZIP:** Show the extraction menu or window.

2. Locate the extracted resource folder. It should be named exactly:

   ```text
   sonoran-safety-vests
   ```

   > **Screenshot placeholder — Extracted resource:** Show the extracted `sonoran-safety-vests` folder.

3. Copy the complete folder directly into your server's `resources` directory:

   ```text
   server-data/
   └── resources/
       └── sonoran-safety-vests/
   ```

   Do not create an extra nested folder such as `sonoran-safety-vests/sonoran-safety-vests/`. The resource manifest should be directly inside the resource folder.

   > **Screenshot placeholder — Resources folder:** Show `sonoran-safety-vests` alongside the server's other resources.

   > **Screenshot placeholder — Correct nesting:** Show the resource manifest inside `resources/sonoran-safety-vests/`.

4. Open `server.cfg` and add:

   ```cfg
   ensure sonoran-safety-vests
   ```

   > **Screenshot placeholder — server.cfg:** Show the `ensure sonoran-safety-vests` line.

5. Save the file and fully restart the FiveM server.

   > **Screenshot placeholder — Server restart:** Show the server being restarted in txAdmin, a hosting panel, or a terminal.

6. Check the startup console for resource errors.

   > **Screenshot placeholder — Startup console:** Show the Safety Vests resource loading without errors.

## Locate the Safety Vests In-Game

The Safety Vests are accessed as EUP clothing through vMenu.

1. Join the server with a character that can access vMenu's clothing options.
2. Open **vMenu**.
3. Open **EUP / Player Appearance**.
4. Navigate to the confirmed clothing category for the Safety Vests.
5. Scroll to the confirmed slot range and select a vest.
6. Use the texture/variation controls to preview the available options.

> **Screenshot placeholder — Open vMenu:** Show the vMenu clothing or player appearance menu.

> **Screenshot placeholder — Clothing category:** Show the exact category where the Safety Vests appear.

> **Screenshot placeholder — Vest slots:** Show the exact slot range with the Safety Vests visible.

> **Screenshot placeholder — Equipped vest:** Show the vest equipped on a player character.

{% hint style="warning" %}
The exact category and slot numbers were not present in the current documentation package. Confirm them in-game with the current resource version, then replace the placeholders in this section and on the landing page.
{% endhint %}

## EUP Streamed-Asset Troubleshooting

If the resource starts but the Safety Vests do not appear, appear invisible, or cause players to disconnect or crash when selected, the issue may be related to FiveM streamed EUP assets or server limits.

### 1. Confirm the resource installation

* Confirm the folder is named `sonoran-safety-vests`.
* Confirm it is directly inside the server's `resources` folder.
* Confirm `server.cfg` contains `ensure sonoran-safety-vests`.
* Confirm the server was fully restarted after installation.
* Confirm the startup console does not report a missing manifest or failed file load.

> **Screenshot placeholder — Resource structure check:** Show the correct folder name and location.

### 2. Check the Cfx.re Element Club subscription

Some EUP streamed assets require an active Cfx.re **Element Club** subscription. If the Safety Vests are installed correctly but the streamed clothing assets do not load, verify that the server owner's Element Club subscription is active and associated with the account used for the server's Cfx.re license/key.

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Check the server owner's Element Club subscription status.
3. Confirm the subscription is active for the account associated with the server license/key.
4. Restart the server after correcting the subscription or account association.

> **Screenshot placeholder — Element Club status:** Show an active Element Club subscription. Redact account IDs, license keys, payment details, and other private information.

{% hint style="warning" %}
Never post private license keys, account information, or payment details in support screenshots.
{% endhint %}

### 3. Temporarily set the maximum players to 10

If the EUP assets still do not stream, temporarily set the server's maximum player count to 10. Add or update this line in `server.cfg`:

```cfg
sv_maxclients 10
```

Fully restart the server and test the Safety Vests again.

> **Screenshot placeholder — Max players in server.cfg:** Show `sv_maxclients 10` in the configuration file.

> **Screenshot placeholder — txAdmin limit:** If applicable, show the matching maximum-player setting set to 10 in txAdmin or your hosting panel.

{% hint style="info" %}
Use the 10-player limit as a troubleshooting step. After the streamed assets work correctly, test a higher player count again if your server setup and Cfx.re requirements support it.
{% endhint %}

### 4. Clear client cache and reconnect

After changing streamed assets or server settings:

1. Stop FiveM.
2. Clear the FiveM client cache using your normal cache-cleaning procedure. Do not delete the entire FiveM installation.
3. Restart FiveM and reconnect to the server.
4. Open vMenu and check the confirmed Safety Vest category and slots again.

> **Screenshot placeholder — Client cache:** Show the cache-cleaning step used by your support procedure.

### 5. Test for EUP resource conflicts

Temporarily disable other recently added EUP or clothing resources and restart the server. If the Safety Vests appear, re-enable the other resources one at a time to identify the conflict.

> **Screenshot placeholder — Resource isolation:** Show the server resource list with other EUP resources disabled for a test restart.

### 6. Review client and server logs

Look for errors mentioning:

* `sonoran-safety-vests`
* streamed assets
* EUP clothing components
* asset or archetype limits
* texture or drawable loading

When requesting support, include the relevant error text, your FiveM build, vMenu version, and a list of other EUP resources. Redact license keys and personal account information first.

> **Screenshot placeholder — Error log:** Show the relevant client or server error with private information redacted.

## Quick Verification Checklist

* [ ] The folder is named `sonoran-safety-vests`.
* [ ] The folder is directly inside the server's `resources` directory.
* [ ] `server.cfg` contains `ensure sonoran-safety-vests`.
* [ ] The server was fully restarted.
* [ ] The resource has no startup errors.
* [ ] vMenu opens successfully.
* [ ] The confirmed EUP clothing category is available.
* [ ] The Safety Vests are visible in the confirmed slot range.
* [ ] If assets do not stream, Element Club status was checked and `sv_maxclients 10` was tested.
