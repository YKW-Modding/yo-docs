---
title: Map IDs
layout: default
has_children: true
parent: Modding Resources
---

# Map IDs
## Format
> Note: some test maps e.g. `testmapl` don't follow this format. They are rare exceptions to this otherwise consistent pattern.

### First Character
This will always be either an `e` or a `t`. If it's an `e`, it's used for events or cutscenes. All other maps are prefixed with `t`.

### Second Character
This character is always a number. It is the zone that the map is in. The zone numbers change between games, so they will be listed below.

#### YW1

| Number | Zone                                              |
| ------ | ----                                              |
|1       | Everything except for the Yo-kai World's Big Map. |
|2       | The Yo-kai World's Big Map.                       |

#### YW2

|Number|Zone|
|------|----|
|0     |The Yo-kai World|
|1     |Present Overworld|
|2     |Past Overworld|
|3     |Sawayama Castle Town|

#### YW3

|Number|Zone|
|------|----|
|0     |The Yo-kai World|
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
