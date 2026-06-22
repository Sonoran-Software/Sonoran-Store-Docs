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
resources/[trafficengine]/sonoran-trafficengine

```

* ![](<../.gitbook/assets/image (23).png>)

</details>

<details>

<summary>Step 2: Ensure the Resource</summary>

Ensure and start Traffic Engine to your server's `server.cfg` file.

```
ensure sonoran-trafficengine
```

* ![](<../.gitbook/assets/image (22).png>)

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
*   These values can be edited as a baseline in the config.lua as a blanket while the regions in game can have their own values.&#x20;

    <figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Evening Rush Multiplier

* How dense will the traffic in this region become during the morning rush hour time?
* &#x20;These values can be edited as a baseline in the config.lua as a blanket while the regions in game can have their own values.  As seen as above.&#x20;

<div><figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure></div>



4. Place Nodes

* Add nodes by placing them down by pressing E to draw out your zone. Press Enter to confirm&#x20;



*   &#x20;Regions are substantially taller and deeper than a zone.

    <figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Zone Creation</summary>

To create zones inside of the region:

1. [Open the menu](getting-started.md#traffic-engine-menu) with `/trafficengine`
2. Select **Create Category** > **Zone**
3. Configure the zone's settings:

Breakdown Allowance

*

    <figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Debris Allowance

*

    <figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>



4. Place Spawn Zones

* Configure spawn points for the breakdowns and debris by pressing **E**, to save press **Enter**.
*   When blue nodes appear those are temporary locations, you can remove or save their location by going to a new area then press E

    <figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

</details>

<details>

<summary>Region and Zone Deletion</summary>



*

    <figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>
* From the in-game menu you will find a Delete section, click on the delete segment to see the Regions and zones,  \
  From there you can delete  Zones and Regions at will, do note "make a backup" before making any changes the changes will save automatically as you make these in game.&#x20;

</details>

Zones will talk to the Region as a Parent to get the data to influence an area for its traffic and events like breakdowns and debris

### Debris

<details>

<summary>Configuring Debris Spawning</summary>



* Steps (Manual + Automatic)
*   If you want to manually add new Debris nodes in a zone, you can press the prompt to spawn node (Yellow) and the Zone will direct its RNG chance to spawn a debris event in that area before the rest of the road.  Heading shows the direction where the debris is facing.

    <figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>
*

    <figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

    Chance = 1.0 This is 100% chance to spawn,  Lower this to have a baseline of how common to see them.\
    \
    Chanceincreasepermiss : This is for when the server does a in game timer to spawn as a cooldown, every 100 seconds in game it rolls a chance to spawn, if the timer game rejects the spawn chance, adds a 10% chance to the spawn,  chance the Chance to .10 for 10% then add .10 for 10% added chance for new spawns in the region for derbis.&#x20;

</details>

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Cleanup Debris</summary>

<figure><img src="../.gitbook/assets/image (4).png" alt="" width="375"><figcaption></figcaption></figure>

Roadway debris can be cleaned up and removed by interacting and holding **E** on the debris.

This allows law enforcement, sanitation, DOT, civilians, or anyone else to clear debris from the roadways.

</details>

### Breakdowns

<details>

<summary>Vehicle Breakdowns</summary>



* ![](<../.gitbook/assets/image (18).png>) ![](<../.gitbook/assets/image (19).png>)



* How to manually spawn a breakdown\
  \
  ![](<../.gitbook/assets/image (20).png>)\
  \
  Each Breakdown node has its own severity with how many cars and emergency services spawn based on chance in config file. &#x20;
* Breakdown nodes manually placed must be inside a zone to be able to spawn.&#x20;
* Breakdowns do slow the speed of traffic substantially. These will cause traffic jams if players are in the area.&#x20;

</details>

### Emergency Response

<details>

<summary>Breakdown Emergency Response</summary>

Vehicle breakdowns can optionally have fire and EMS emergency vehicles respond for added realism.

Emergency Nodes will show up in blue when manually placed.

* ![](<../.gitbook/assets/image (16).png>)
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

Go to config.lua and find this value to apply Override or not depending on what traffic manager you use.&#x20;

![](<../.gitbook/assets/image (15).png>)

</details>
