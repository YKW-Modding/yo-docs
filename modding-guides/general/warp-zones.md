---
title: How to Edit Map Jumps in Yo-kai Watch 2 and 3
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# How to Edit Map Jumps in Yo-kai Watch 2 and 3
> **Original guide by @lit_8 on Discord. Slightly modified by @n123original**. This guide assumes you already know how to navigate romfs and use CfgBin Editor. If not, please read [the starting guide](../gettingstarted.html). Additionally, this only works on map jumps, so normal entrances and some basic doors. No XQ controlled jumps so, no doors that require items, no elevators, and no trains.

* First, extract and open `mapenv.pck` from `data/map/<MAP>` in Kuriimu2.
* Next, extract `<MAP>_funcpt.bin` from it, and open it in CfgBin Editor.
* Next, search through the `PTREE` trees with `MAP_JUMP` parameters, until you find the one you want
* Next, edit the MapID on the right of the string and the number if needed to make the string unique.
  * For example, you might change `MJ_t101g00_t106g00_01` to `MJ_t101g00_t102g00_01` or `MJ_t101g00_t102g00_02`, if the 1st option already exists.
* Finally, save and replace it back in the `mapenv.pck`.
