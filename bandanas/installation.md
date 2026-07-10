---
description: >-
  Install Sonoran's pocket bandanas with real movement — built to flow naturally
  with every step.
---

# Installation

This guide covers downloading, installing, and locating the Sonoran Bandanas EUP accessory pack.

{% hint style="info" %}
The resource must be named `sonoran-bandanas` and the resource must be started with `ensure sonoran-bandanas`.
{% endhint %}

## Download the Resource

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Open the purchased assets or packages area for the account that owns the Bandanas pack.
3. Download the Bandanas `.zip` file to your computer.

<div><figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption><p>CFX Portal - Download</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption><p>Windows - Downloaded ZIP File</p></figcaption></figure></div>

## Install the Resource

1.  Extract or unzip the downloaded file.

    <div><figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption><p>Windows - Extract Zip File</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption><p>Windows - Choose Extract Location</p></figcaption></figure></div>
2.  Open the extracted package and locate the resource folder. The folder should be named exactly:

    ```
    sonoran-bandanas
    ```

    <figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>
3.  Copy the complete `sonoran-bandanas` folder into your server's `resources` directory. A typical layout is:

    ```
    server-data/
    └── resources/
        └── sonoran-bandanas/
    ```

    Do not put the folder inside an additional nested folder. The resource manifest should be directly inside `resources/sonoran-bandanas/`.

    <div><figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption><p>Pterodactyl - Uploaded FIle</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption><p>Pterodactyl - <code>sonoran-bandanas</code> folder</p></figcaption></figure></div>
4.  Open your `server.cfg` and add the following line:

    ```cfg
    ensure sonoran-bandanas
    ```

    Place it with the other resources that are started when the server boots.

    <figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>
5.  Save `server.cfg` and fully restart the FiveM server.

    <figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>
6.  Check the server console during startup. You should not see a missing resource, manifest, or file error for `sonoran-bandanas`.

    <figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

## Locate the Bandanas In-Game

The Bandanas pack is available as an EUP accessory through vMenu.

1. Join the server with a character that can access vMenu's clothing options.
2. Open **vMenu**.
3. Go to the **EUP / Player Appearance** clothing menu.
4. Open the **Neck and Scarves** category.
5. Scroll to the end of the category. The Bandanas are located in slots **200–208** on vMenu.
6. Select a slot to preview and equip the bandana. Use the texture/variation controls to cycle through the available designs.

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

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

### 2. Check your Cfx.re Element Club subscription

Some EUP streamed assets require an active Cfx.re **Element Club** subscription. If the resource is installed correctly but streamed clothing assets are not loading, verify that the server owner account has the required Element Club subscription for the EUP assets being used.

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Check the server owner's Element Club subscription status.
3. Confirm the subscription is active on the account associated with the server's Cfx.re license/key.
4. Restart the server after the subscription or account association is corrected.

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Do not post private license keys, account information, or payment details in support channels or screenshots.
{% endhint %}

### 3. Set the server to a maximum of 10 players

If EUP assets still fail to stream, temporarily set the server's maximum player count to 10. Add or update this line in `server.cfg`:

```cfg
sv_maxclients 10
```

Then fully restart the server and test the Bandanas again.

<div><figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption><p>TxAdmin - Setting Max Players to 10</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption><p>RocketNode Panel - Setting Max Players to 10 </p></figcaption></figure></div>

{% hint style="info" %}
Use the 10-player limit as a troubleshooting step while testing streamed EUP assets. Once the assets are working, you can test a higher player count again if your server setup and Cfx.re requirements support it.
{% endhint %}

### 4. Clear client cache and test again

After changing streamed assets or server settings:

1. Stop FiveM.
2. Clear the FiveM client cache using your normal FiveM cache-cleaning procedure. Do not delete your entire FiveM installation.
3. Restart FiveM and reconnect to the server.
4. Open vMenu and check **Neck and Scarves**, slots **200–208**, again.

### 5. Test for resource conflicts

Temporarily test with other recently added EUP or clothing resources disabled. If the Bandanas appear after another resource is disabled, re-enable resources one at a time to identify the conflict.

### 6. Review the client and server logs

Look for errors mentioning:

* `sonoran-bandanas`
* streamed assets
* EUP clothing components
* asset or archetype limits
* texture or drawable loading

When requesting support, include the relevant error text, your FiveM build, vMenu version, and a list of other EUP resources. Remove license keys and personal account information first.

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
