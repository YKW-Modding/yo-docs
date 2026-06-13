---
title: Making Custom Events Work Properly
layout: default
parent: Creating Your Own Postgame Stories
grand_parent: Modding Guides
---

# Making Custom Events Work Properly
> Original author on Discord: @z_u_ra, modified by @n123original. This guide assumes you already know how to navigate romfs and use CfgBin Editor. If not, please read [the starting guide](../gettingstarted.html).

Sometimes, adding an event won't work. Here's a potential solution:

* First, open `event_set_config*.cfg.bin` from `data/res/sys/`.
  * The `*` refers to versioning, so instead of just `event_set_config.cfg.bin` you might also see files such as `event_set_config_0.01.cfg.bin`. Pick the one with the highest version.
* Next, duplicate an `EVENT_SET_CONFIG_*` entry, the new entry should appear at the bottom of the tree.
* In the `EventID` parameter, place the ID of your event e.g. the CRC-32 of `ev12_3400`.
* Configure all the documented variables to your liking and leave all of the undocumented variables as is.
* Finally, increment (increase by 1) the `ChildCount` of the `EVENT_SET_CONFIG_LIST` tree that contains the entries.

If that still doesn't work, try adding a `GlobalBitFlag` with the `EventID` as the `FlagID`, you can do so via [this tutorial](add-flags.html).
