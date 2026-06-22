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

    <figure><img src="../.gitbook/assets/image (12).png" alt="" width="375"><figcaption></figcaption></figure>

</details>

<details>

<summary>Region and Zone Deletion</summary>

Delete zones or regions from the in-game menu.

Note that any changes will be saved automatically, save a backup file if you wish to revert your changes.

*

    <figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

</details>

### Debris

<details>

<summary>Configuring Debris Spawning</summary>

Debris will automatically spawn inside the zone depending on the maximum amount of debris allowance set when [first creating the zone](getting-started.md#zone-creation).

Debris spawn frequency will be based on the `config.Debris` `chance` value.

Users can also manually spawn debris at a specific location in a zone via the in-game menu. To manually spawn a debris, navigate to **Spawn Category** > **Debris**.

<div><figure><img src="../.gitbook/assets/image (13).png" alt="" width="298"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/image (14).png" alt="" width="272"><figcaption></figcaption></figure></div>

The `Config.Debris` values can be tweaked for further customization.

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

`tickIntervalMs` - How often the breakdown system checks whether it should try to spawn one.

`spawnCooldownMs` - Cooldown after a successful breakdown before another normal spawn can happen.

`guaranteedSpawnIntervalMs` - Maximum time allowed without a breakdown before the system forces a guaranteed attempt.

`eventActiveSeconds` - How long a breakdown stays active before expiring.

`nodeGracePeriodSeconds` - How long a used breakdown node stays locked before it can be reused.

`minPlayerDistance` - Minimum distance a player must be from the node for a breakdown to spawn.

`cleanupTimeMs` - Cleanup timer used to remove the breakdown scene after it expires.

`cleanupWhenPlayersFar` - If enabled, cleans up breakdowns early when players move far enough away.

`useRoadBlockingFallback` - If enabled, blocks road pathing around the breakdown when needed.

`allowPedSpawnWithBreakdown` - If enabled, allows a stranded ped to spawn with the breakdown vehicle.&#x20;

</details>

<details>

<summary>Cleanup Debris</summary>

<figure><img src="../.gitbook/assets/image (4).png" alt="" width="375"><figcaption></figcaption></figure>

Roadway debris can be cleaned up and removed by interacting and holding **E** on the debris.

This allows law enforcement, sanitation, DOT, civilians, or anyone else to clear debris from the roadways.

</details>

### Breakdowns

<details>

<summary>Vehicle Breakdowns</summary>

Breakdowns will automatically spawn inside the zone depending on the maximum amount of breakdown allowance set when [first creating the zone](getting-started.md#zone-creation).

* ![](<../.gitbook/assets/image (18).png>) ![](<../.gitbook/assets/image (19).png>)



Breakdown spawn frequency will be based on the `config.Breakdowns` `chance` value.

Users can also manually spawn breakdowns at a specific location in a zone via the in-game menu. To manually spawn a breakdown, navigate to **Spawn Category** > **Breakdown**.

<figure><img src="../.gitbook/assets/image (20).png" alt="" width="300"><figcaption></figcaption></figure>

When a breakdown spawns, it can be one of several severity scenarios. Higher level breakdowns can also spawn [emergency services](getting-started.md#emergency-response) that will respond to it.

Breakdowns slow the speed of traffic substantially, causing traffic jams if players are in the area.&#x20;

Config.Breakdowns:

```
enabled = true,
```

Turns the breakdown system on or off.

```
chance = 1.00,
```

Base chance for a breakdown event to occur when the system rolls.

```
chanceIncreasePerMiss = 0.08,
```

Adds 8% to the breakdown chance after each failed roll.

```
rushHourChanceIncreaseBonus = 0.20,
```

Adds an extra 20% chance during configured rush-hour periods.

```
maxActive = 500,
```

Maximum number of active breakdowns allowed server-wide.

```
maxActivePerZone = 10,
```

Maximum number of active breakdowns allowed in a single zone.

```
maxActivePerRegion = 30,
```

Maximum number of active breakdowns allowed in a single region.

```
trafficBlockRadius = 30.0,
```

Distance around the breakdown where AI traffic will react and avoid the scene.

</details>

### Emergency Response

<details>

<summary>Breakdown Emergency Response</summary>

When a high level [vehicle breakdown](getting-started.md#breakdowns) spawns, fire and EMS vehicles can also spawn nearby for added realism.

Users can also manually spawn emergency vehicles at a specific location in a zone via the in-game menu. To manually spawn an emergency vehicle, navigate to **Spawn Category** > **Emergency**. Emergency Nodes will show up in blue when manually placed.

<figure><img src="../.gitbook/assets/image (16).png" alt="" width="262"><figcaption></figcaption></figure>

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



```lua
Config.CalmAI = {
    -- MASTER TOGGLE FOR CALM AI POPULATION OVERRIDES. | BOOLEAN | TRUE OR FALSE
    enabled = true,
    -- HOW MANY VEHICLES ARE DRIVING AROUND THE MAP. LOWER = LESS MOVING TRAFFIC. | NUMBER | 0.0 TO 3.0
    VehDensity = 0.85,
    -- HOW MANY RANDOMLY SPAWNED TRAFFIC VEHICLES ARE AROUND. LOWER = LESS RANDOM TRAFFIC. | NUMBER | 0.0 TO 3.0
    RanVehDensity = 0.65,
    -- HOW MANY PARKED CARS ARE AROUND THE MAP. LOWER = FEWER PARKED CARS. | NUMBER | 0.0 TO 3.0
    ParkCarDensity = 0.95,
    -- HOW MANY WALKING/STREET AI PEDS ARE AROUND. LOWER = FEWER PEDS. | NUMBER | 0.0 TO 1.0
    PedDensity = 0.75,
    -- HOW MANY SCENARIO PEDS ARE RUNNING EMOTES/TASKS. LOWER = FEWER SCENE PEDS. | NUMBER | 0.0 TO 1.0
    ScenePedDensity = 0.35,
    -- TRUE DISABLES VANILLA AI DISPATCH. FALSE LEAVES VANILLA DISPATCH ON. | BOOLEAN | TRUE OR FALSE
    DispatchDead = true
```

</details>
