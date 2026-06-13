---
title: How to Add Custom Fusions
layout: default
grand_parent: Modding Guides
parent: General Modding
---
> **Original guide by @whisperito on discord, modified by @n123original. This guide assumes you already know how to navigate romfs and use CfgBin Editor. If not, please read [the starting guide](../gettingstarted.html). Additionally, **this guide does not work on B1 and B2.**

## How to Add Custom Fusions
* First, extract and open `combine_config.cfg.bin` from `data/res/shop`.
* Next, duplicate a `COMBINE_INFO_*` entry, the new entry should appear at the end of the tree.
* Next, configure the parameters
  * `IsXYZItem` parameters should be 1 if `XYZ` is an Item, else 0 if it's a Yo-kai.
  * The ID parameters should be the `ItemID` if an item, `ParamID` if a Yo-kai.
* Finally, increment (increase by 1) the `ChildCount` of the `COMBINE_INFO` tree that contains the entries.
