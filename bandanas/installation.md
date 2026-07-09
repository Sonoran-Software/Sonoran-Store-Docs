# Bandanas: Installation & In-Game Location

This guide covers downloading, installing, and locating the Sonoran Bandanas EUP accessory pack.

{% hint style="info" %}
The resource must be named `sonoran-bandanas` and the resource must be started with `ensure sonoran-bandanas`.
{% endhint %}

## Before You Begin

You will need:

* Access to the Cfx.re Portal account that purchased the product.
* Access to your FiveM server's `resources` folder.
* Access to your server's `server.cfg` file.
* A way to restart the server after installation.

> **Screenshot placeholder — Cfx.re Portal:** Show the purchased Bandanas asset in the Cfx.re Portal and the download button.

## Download the Resource

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Open the purchased assets or packages area for the account that owns the Bandanas pack.
3. Download the Bandanas `.zip` file to your computer.

> **Screenshot placeholder — Download:** Show the Bandanas `.zip` download in the Cfx.re Portal.

> **Screenshot placeholder — Downloaded file:** Show the downloaded `.zip` file in the computer's Downloads folder.

## Install the Resource

1. Extract or unzip the downloaded file.

   > **Screenshot placeholder — Extract ZIP:** Show the context menu or extraction window used to unzip the downloaded package.

2. Open the extracted package and locate the resource folder. The folder should be named exactly:

   ```text
   sonoran-bandanas
   ```

   > **Screenshot placeholder — Extracted folder:** Show the extracted `sonoran-bandanas` folder before it is moved to the server.

3. Copy the complete `sonoran-bandanas` folder into your server's `resources` directory. A typical layout is:

   ```text
   server-data/
   └── resources/
       └── sonoran-bandanas/
   ```

   Do not put the folder inside an additional nested folder. The resource manifest should be directly inside `resources/sonoran-bandanas/`.

   > **Screenshot placeholder — Server resources:** Show `sonoran-bandanas` alongside the server's other resources.

   > **Screenshot placeholder — Correct nesting:** Show the resource manifest inside `resources/sonoran-bandanas/`, with no extra `sonoran-bandanas/sonoran-bandanas/` layer.

4. Open your `server.cfg` and add the following line:

   ```cfg
   ensure sonoran-bandanas
   ```

   Place it with the other resources that are started when the server boots.

   > **Screenshot placeholder — server.cfg:** Show the `ensure sonoran-bandanas` line in the server configuration.

5. Save `server.cfg` and fully restart the FiveM server.

   > **Screenshot placeholder — Server restart:** Show the server being restarted in the host panel, txAdmin, or terminal.

6. Check the server console during startup. You should not see a missing resource, manifest, or file error for `sonoran-bandanas`.

   > **Screenshot placeholder — Startup console:** Show a successful server startup with the Bandanas resource loading.

## Locate the Bandanas In-Game

The Bandanas pack is available as an EUP accessory through vMenu.

1. Join the server with a character that can access vMenu's clothing options.
2. Open **vMenu**.
3. Go to the **EUP / Player Appearance** clothing menu.
4. Open the **Neck and Scarves** category.
5. Scroll to the end of the category. The Bandanas are located in slots **200–208** on vMenu.
6. Select a slot to preview and equip the bandana. Use the texture/variation controls to cycle through the available designs.

> **Screenshot placeholder — Open vMenu:** Show the vMenu clothing or player appearance menu.

> **Screenshot placeholder — Neck and Scarves:** Show the **Neck and Scarves** category selected.

> **Screenshot placeholder — Slots 200–208:** Show the end of the category with slots 200 through 208 visible.

> **Screenshot placeholder — Equipped bandana:** Show the bandana equipped on a player character in-game.

{% hint style="info" %}
The exact appearance of the menu can vary slightly depending on your vMenu version and other EUP resources installed on the server. The Bandanas should still be at the end of **Neck and Scarves**, in slots **200–208**.
{% endhint %}

## EUP Streamed-Asset Troubleshooting

If the resource starts but the Bandanas do not appear, appear invisible, or cause players to disconnect or crash when selecting them, the issue is usually related to FiveM streamed EUP asset limits rather than the `ensure` line.

### 1. Confirm the resource is installed correctly

* Confirm the folder is named `sonoran-bandanas`.
* Confirm it is directly inside the server's `resources` folder.
* Confirm `server.cfg` contains `ensure sonoran-bandanas`.
* Confirm the server was fully restarted after the resource was added.
* Confirm the server console does not report a missing manifest or failed file load.

> **Screenshot placeholder — Resource structure check:** Show the correct folder name and location after troubleshooting.

### 2. Check your Cfx.re Element Club subscription

Some EUP streamed assets require an active Cfx.re **Element Club** subscription. If the resource is installed correctly but streamed clothing assets are not loading, verify that the server owner account has the required Element Club subscription for the EUP assets being used.

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Check the server owner's Element Club subscription status.
3. Confirm the subscription is active on the account associated with the server's Cfx.re license/key.
4. Restart the server after the subscription or account association is corrected.

> **Screenshot placeholder — Element Club status:** Show an active Element Club subscription in the Cfx.re Portal. Hide account IDs, payment details, and other private information.

{% hint style="warning" %}
Do not post private license keys, account information, or payment details in support channels or screenshots.
{% endhint %}

### 3. Set the server to a maximum of 10 players

If EUP assets still fail to stream, temporarily set the server's maximum player count to 10. Add or update this line in `server.cfg`:

```cfg
sv_maxclients 10
```

Then fully restart the server and test the Bandanas again.

> **Screenshot placeholder — Max players:** Show `sv_maxclients 10` in `server.cfg`.

> **Screenshot placeholder — txAdmin player limit:** If your server is managed through txAdmin or a hosting panel, show the matching maximum-player setting set to 10.

{% hint style="info" %}
Use the 10-player limit as a troubleshooting step while testing streamed EUP assets. Once the assets are working, you can test a higher player count again if your server setup and Cfx.re requirements support it.
{% endhint %}

### 4. Clear client cache and test again

After changing streamed assets or server settings:

1. Stop FiveM.
2. Clear the FiveM client cache using your normal FiveM cache-cleaning procedure. Do not delete your entire FiveM installation.
3. Restart FiveM and reconnect to the server.
4. Open vMenu and check **Neck and Scarves**, slots **200–208**, again.

> **Screenshot placeholder — Client cache:** Show the cache location or the cache-cleaning step used by your server's support procedure.

### 5. Test for resource conflicts

Temporarily test with other recently added EUP or clothing resources disabled. If the Bandanas appear after another resource is disabled, re-enable resources one at a time to identify the conflict.

> **Screenshot placeholder — Resource isolation:** Show the server resource list with other EUP resources disabled for a test restart.

### 6. Review the client and server logs

Look for errors mentioning:

* `sonoran-bandanas`
* streamed assets
* EUP clothing components
* asset or archetype limits
* texture or drawable loading

When requesting support, include the relevant error text, your FiveM build, vMenu version, and a list of other EUP resources. Remove license keys and personal account information first.

> **Screenshot placeholder — Error log:** Show the relevant client or server error with private information redacted.

## Quick Verification Checklist

Use this checklist after installation:

* [ ] The folder is named `sonoran-bandanas`.
* [ ] The folder is directly inside the server's `resources` directory.
* [ ] `server.cfg` contains `ensure sonoran-bandanas`.
* [ ] The server was fully restarted.
* [ ] The resource has no startup errors.
* [ ] vMenu opens successfully.
* [ ] **Neck and Scarves** is available in the EUP menu.
* [ ] The Bandanas are visible in slots **200–208**.
* [ ] If assets do not stream, Element Club status was checked and `sv_maxclients 10` was tested.
