# Installation

This guide covers downloading, installing, and locating the Sonoran Safety Vests EUP items.

{% hint style="info" %}
The resource folder should be named `sonoran-safetyvest` and started with `ensure sonoran-safetyvest`.
{% endhint %}

{% hint style="info" %}
This EUP asset is escrow protected by Tebex. [Learn more about what this means.](installation.md#texture-customization)
{% endhint %}

## Before You Begin

You will need:

* Access to the Cfx.re Portal account that purchased the Safety Vests.
* Access to your FiveM server's `resources` folder.
* Access to your server's `server.cfg` file.
* A way to fully restart the server after installation.

## Download the Resource

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Open the purchased assets or packages area for the account that owns the Safety Vests.
3. Download the Safety Vests `.zip` file.

<div><figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption><p>CFX Portal - Download</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption><p>Windows - Downloaded ZIP File</p></figcaption></figure></div>

## Install the Resource

1. Extract or unzip the downloaded file.

<div><figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption><p>Windows - Extract Zip File</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure></div>

1.  Locate the extracted resource folder. It should be named exactly:

    ```
    sonoran-safetyvest
    ```

    <figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>
2.  Copy the complete folder directly into your server's `resources` directory:

    ```
    server-data/
    └── resources/
        └── sonoran-safetyvest/
    ```

    Do not create an extra nested folder such as `sonoran-safetyvest/sonoran-safetyvest/`. The resource manifest should be directly inside the resource folder.

<div><figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption><p>Pterodactyl - Uploaded FIle</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (55).png" alt=""><figcaption><p>Pterodactly - <code>sonoran-safetyvest</code> folder</p></figcaption></figure></div>

1.  Open `server.cfg` and add:

    ```cfg
    ensure sonoran-safetyvest
    ```

    <figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>
2.  Save the file and fully restart the FiveM server.

    <figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>
3.  Check the startup console for resource errors.

    <figure><img src="../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

## Locate the Safety Vests In-Game

The Safety Vests are accessed as EUP clothing through vMenu.

1. Join the server with a character that can access vMenu's clothing options.
2. Open **vMenu**.
3. Open **EUP / Player Appearance**.
4. Open **Shirt and Accessory**.
5. Scroll all the way to the end of the category and select slot **217**.
6. Use the texture/variation controls to preview the available options.

## EUP Streamed-Asset Troubleshooting

If the resource starts but the Safety Vests do not appear, appear invisible, or cause players to disconnect or crash when selected, the issue may be related to FiveM streamed EUP assets or server limits.

### 1. Confirm the resource installation

* Confirm the folder is named `sonoran-safetyvest`.
* Confirm it is directly inside the server's `resources` folder.
* Confirm `server.cfg` contains `ensure sonoran-safetyvest`.
* Confirm the server was fully restarted after installation.
* Confirm the startup console does not report a missing manifest or failed file load.

### 2. Check the Cfx.re Element Club subscription

Some EUP streamed assets require an active Cfx.re **Element Club** subscription. If the Safety Vests are installed correctly but the streamed clothing assets do not load, verify that the server owner's Element Club subscription is active and associated with the account used for the server's Cfx.re license/key.

1. Sign in to the [Cfx.re Portal](https://portal.cfx.re/).
2. Check the server owner's Element Club subscription status.
3. Confirm the subscription is active for the account associated with the server license/key.
4. Restart the server after correcting the subscription or account association.

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Never post private license keys, account information, or payment details in support screenshots.
{% endhint %}

### 3. Temporarily set the maximum players to 10

If the EUP assets still do not stream, temporarily set the server's maximum player count to 10. Add or update this line in `server.cfg`:

```cfg
sv_maxclients 10
```

Fully restart the server and test the Safety Vests again.

<div><figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption><p>TxAdmin - Setting Max Players to 10</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption><p>RocketNode Panel - Setting Max Players to 10</p></figcaption></figure></div>

{% hint style="info" %}
Use the 10-player limit as a troubleshooting step. After the streamed assets work correctly, test a higher player count again if your server setup and Cfx.re requirements support it.
{% endhint %}

### 4. Clear client cache and reconnect

After changing streamed assets or server settings:

1. Stop FiveM.
2. Clear the FiveM client cache using your normal cache-cleaning procedure. Do not delete the entire FiveM installation.
3. Restart FiveM and reconnect to the server.
4. Open vMenu and check **Player Appearance → Shirt and Accessory**, slot **217**, again.

### 5. Test for EUP resource conflicts

Temporarily disable other recently added EUP or clothing resources and restart the server. If the Safety Vests appear, re-enable the other resources one at a time to identify the conflict.

### 6. Review client and server logs

Look for errors mentioning:

* `sonoran-safetyvest`
* streamed assets
* EUP clothing components
* asset or archetype limits
* texture or drawable loading

When requesting support, include the relevant error text, your FiveM build, vMenu version, and a list of other EUP resources. Redact license keys and personal account information first.

## Quick Verification Checklist

* [ ] The folder is named `sonoran-safetyvest`.
* [ ] The folder is directly inside the server's `resources` directory.
* [ ] `server.cfg` contains `ensure sonoran-safetyvest`.
* [ ] The server was fully restarted.
* [ ] The resource has no startup errors.
* [ ] vMenu opens successfully.
* [ ] **Player Appearance → Shirt and Accessory** is available.
* [ ] The Safety Vest is visible at slot **217**, at the end of the category.
* [ ] If assets do not stream, Element Club status was checked and `sv_maxclients 10` was tested.

## Escrow Protection

Sonoran’s Safety Vest EUP is protected through Tebex and Cfx.re Asset Escrow. This helps prevent unauthorized redistribution, leaks, and resale.

#### Runs as a Separate Resource

Because the asset is escrow-protected, the Safety Vest EUP must run as its own separate resource and cannot be merged into an existing clothing pack.

The resource is heavily optimized for smooth performance, and textures can still be customized.

#### Texture Customization

This EUP cannot be edited through Durty Cloth Tool due to Asset Escrow restrictions.

Textures can still be modified by opening the included `.ytd` file in OpenIV.
