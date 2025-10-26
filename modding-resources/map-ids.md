---
title: Map IDs
layout: default
has_children: true
parent: Modding Resources
---

# Map IDs

## Format

### First Character

This will always be either an `e` or a `t`. If it is an `e`, it is a map used in an event or a cutscene. If it is a `t`, it is just a regular map.

### Second Character

This character is always a number. It is the zone that the map is in. The zone numbers change between games, so I will list them here.

**YKW1:**

|Number|Zone|
|------|----|
|1     |Everything except for the Yo-Kai World's Big Map|
|2     |The Yo-Kai World's Big Map|

**YKW2:**

|Number|Zone|
|------|----|
|0     |The Yo-Kai World|
|1     |Present Overworld|
|2     |Past Overworld|
|3     |Sawayama Castle Town|

**YKW3:**

|Number|Zone|
|------|----|
|0     |The Yo-Kai World|
|1     |Overworld|
|2     |Past Overworld *(FULLY UNUSED)*|
|3     |Reserved for events/cutscenes|
|4     |BBQ|
|5     |Blasters T Maps|

### Third and Fourth Characters

Together, these two characters form a two digit number which defines the place that the map is intended for (e.g: `01` for Uptown Springdale).

### Fifth Character

This can either be `g`, `i`, `d`, `w`, `s`, `t`, or `b`.

| Letter | Meaning           |
| ------ | ----------------  |
| g      | "Big" maps        |
| i      | Interiors         |
| d      | Dungeon           |
| w      | Watchmap          |
| b      | Battle background |
| s      | Train station     |
| t      | Train interior    |

### Sixth and Seventh Characters

Together, these two characters form a two digit number which simply is used to order the maps.
