---
description: Get started with configuring Traffic Engine for your community.
---

# Getting Started

## Installation

<details>

<summary>Step 1: Install the Resource</summary>

Place the Traffic Engine resource into your server's resources folder.

Example:

```
resources/[traffic]/traffic_engine
```

TODO

* Image of install

</details>

<details>

<summary>Step 2: Ensure the Resource</summary>

Ensure and start Traffic Engine to your server's `server.cfg` file.

```
ensure sonoran-trafficengine
```

TODO

* Image of ensure line in CFG?

</details>

<details>

<summary>Step 3: Configure Permissions</summary>

Traffic Engine uses ACE permissions for administrative access.

Add the following permission to your server configuration:

```
add_ace group.admin trafficengine.admin allow
```

Example:

```
add_principal identifier.fivem:123456 group.admin
add_ace group.admin trafficengine.admin allow
```

</details>

## Traffic Engine Menu

<details>

<summary>Opening Traffic Engine</summary>

Administrators can access Traffic Engine in-game using:

```
/trafficengine
```

This command opens the Traffic Engine administrative toolset.

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="375"><figcaption></figcaption></figure>

This menu allows for the creation of [regions](getting-started.md#region-creation), [zones](getting-started.md#zone-creation), [breakdowns](getting-started.md#breakdowns), and [debris](getting-started.md#debris) management.

</details>

## Region and Zone Management

<details>

<summary>What are Regions?</summary>

**Regions**

A region represents a large section of the map and acts as the parent for all traffic behavior within that area.

**Example Regions:**

* City Region: `Los Santos`
* County Region: `Blaine County`
* Rural Region: `Paleto Bay`

</details>

<details>

<summary>What are Zones?</summary>

Zones are smaller areas inside a region.

Examples:

* Downtown Los Santos
* Interstate 1
* Airport Access Road
* Mirror Park

Zones allow you to override regional settings and create unique traffic behavior in specific locations.

For example:

| Area               | Morning/Evening Rush Hour Density Value 1-3 |
| ------------------ | ------------------------------------------- |
| Los Santos Region  | 3                                           |
| Legion Square Zone | 2                                           |

</details>

<details>

<summary>Region Creation</summary>

To create a region:

1. [Open the menu](getting-started.md#traffic-engine-menu) with `/trafficengine`
2. Select **Create Category** > **Region**
3. Configure the region's name and rush hour settings:

Morning Rush Multiplier

* How dense will the traffic in this region become during the morning rush hour time?
* TODO: config.what? property

Evening Rush Multiplier

* How dense will the traffic in this region become during the morning rush hour time?
* TODO: config.what? property

<div><figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure></div>



4. Place Nodes

* Add nodes by placing them down by pressing E to draw out your zone. Press Enter to confirm&#x20;

TODO

* Image of drawing the region

</details>

<details>

<summary>Zone Creation</summary>

To create zones inside of the region:

1. [Open the menu](getting-started.md#traffic-engine-menu) with `/trafficengine`
2. Select **Create Category** > **Zone**
3. Configure the zone's settings:

Breakdown Allowance

* TODO: Image

Debris Allowance

* TODO Image



4. Place Spawn Zones

* Configure spawn points for the breakdowns and debris by pressing **E**, to save press **Enter**.
* TODO images of menu to place spawn points, placing points, etc.

</details>

<details>

<summary>Region and Zone Deletion</summary>

TODO

* Images of zone delete
* Images of region delete

</details>

Zones will talk to the Region as a Parent to get the data to influence an area for its traffic and events like breakdowns and debris

### Debris

<details>

<summary>Configuring Debris Spawning</summary>

TODO

* Steps (Manual + Automatic)
* Images (menu + in-game points)
* Config options to tweak chances

</details>

<details>

<summary>Cleanup Debris</summary>

<figure><img src="../.gitbook/assets/image (4).png" alt="" width="375"><figcaption></figcaption></figure>

Roadway debris can be cleaned up and removed by interacting and holding **E** on the debris.

This allows law enforcement, santiation, DOT, civilians, or anyone else to clear debris from the roadways.

</details>

### Breakdowns

<details>

<summary>Vehicle Breakdowns</summary>

TODO:

* Image(s) of breakdowns
* How to configure a breakdown zone - steps + images of menu and in-game points
* How to manually spawn a breakdown

</details>

### Emergency Response

<details>

<summary>Breakdown Emergency Response</summary>

Vehicle breakdowns can optionally have fire and EMS emergency vehicles respond for added realism.

TODO

* Image(s) of emergency response
* How to configure emergency response (just config options?)

</details>

## Calm AI

{% hint style="danger" %}
Traffic Engine will conflict with scripts like **Calm AI** and other traffic management scripts.

Disable any existing traffic management scripts and use only Traffic Engine for these features.
{% endhint %}

<details>

<summary>Calm AI Traffic Management</summary>

Many communities use traffic and AI management systems like "Calm AI". These scripts manage the overall traffic density across the map and AI aggressiveness behavior.

Because Traffic Engine will conflict with any existing Calm AI script, these features are built in.

TODO

* Config options/names for this

</details>
