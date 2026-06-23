---
title: Commands and Usage
published: true
date: 2026-06-23T00:00:00.000Z
tags: null
editor: markdown
dateCreated: 2026-06-23T00:00:00.000Z
description: Command reference and common in-game workflows for Street Signs.
---

# Commands and Usage

## Main Workflows

Street Signs can be used in two main ways:

* Create and manage signs with chat commands
* Walk up to a nearby sign and press `E` to edit it in-game

Administrators can also open the full sign controller.

## Commands

| Command | What it does | Typical access |
| --- | --- | --- |
| `/signcreate [id] [label optional]` | Creates a sign at your current position | Create permission |
| `/signedit [id] [line1\|line2\|label\|theme\|enable\|disable] [value]` | Updates a supported field on an existing sign | Edit permission |
| `/signdelete [id]` | Deletes a sign | Delete permission |
| `/signrefresh` | Broadcasts a full sign refresh to all clients | Admin |
| `/signlist` | Lists loaded signs in chat | Admin |
| `/signsettext [id] [text]` | Replaces the first line of text on a sign | Edit permission |
| `/signcontroller` | Opens the full controller interface | Admin |

## Creating a Sign

Basic example:

```text
/signcreate downtown_001 Downtown Closure
```

This creates a sign at your current location using the script's default sign setup.

## Editing a Sign In-Game

1. Walk close to the sign
2. Press `E`
3. Make your changes in the editor
4. Save your changes

If the player does not have permission to edit the sign, the editor will not open.

## Editing a Sign With Commands

### Update line 1

```text
/signedit downtown_001 line1 ROAD CLOSED
```

### Update line 2

```text
/signedit downtown_001 line2 USE ALT ROUTE
```

### Change the label

```text
/signedit downtown_001 label Downtown Closure Board
```

### Change the theme

```text
/signedit downtown_001 theme amber
```

### Disable a sign

```text
/signedit downtown_001 disable
```

### Enable a sign

```text
/signedit downtown_001 enable
```

## Quick Text Update

Use `/signsettext` when you only want to replace the primary line quickly:

```text
/signsettext downtown_001 ROAD WORK AHEAD
```

## Managing Signs

### Delete a sign

```text
/signdelete downtown_001
```

### View all loaded signs

```text
/signlist
```

### Force a refresh

```text
/signrefresh
```

## Full Controller

The full controller is opened with:

```text
/signcontroller
```

This is intended for administrative or broader management use and is not the same as simply walking up to a single nearby sign.

## Sign Placement Notes

When you create a sign with `/signcreate`, the sign is placed at your current position and heading.

For best results:

* Stand exactly where you want the sign created
* Face the direction you want it oriented
* Use a clear and unique sign ID

## Suggested ID Format

Using a consistent naming style makes long-term management easier.

Examples:

* `downtown_001`
* `i1_detour_west`
* `airport_event_gate_a`
* `paleto_warning_01`

## Common Use Cases

* Temporary road closures
* Construction zones
* Directional event signage
* Traffic advisories
* Highway warning messages
* Persistent world detail
