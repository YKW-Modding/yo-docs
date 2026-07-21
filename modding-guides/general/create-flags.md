---
title: Defining Flags
layout: default
parent: General Modding
grand_parent: Modding Guides
---

# Defining Flags
> Original author for GlobalBitFlags: @z_u_ra. Rewritten and expanded by @n123original. This guide assumes you already know how to navigate romfs and use CfgBin Editor. If not, please read [the starting guide](../gettingstarted.html).

There are several types of flags with the most common type being `GlobalBitFlag`s.
These are *boolean* meaning they can be either:
* `1`; this could mean enabled, true, unlocked, on or any similar state.
* `0`; this means the inverse, so it could mean disabled, false, and so on.
 
You can get/set these in XQ via `get_global_bitflag()` and `set_global_bitflag()`. Another common type is `GlobalByteFlags` which are *8-bit unsigned integers* meaning they can be any number between 0 and 255. I'm not going to cover the meaning, differences, and details of every flag type. I have however in [this](../../modding-resources/flags.html) page.
> I will also NOT cover secondary flag types such as `GlobalTrophyGetFlag`s, as they are reserved for their own guides, e.g. a guide on custom trophies.

## Configuring Flags
First, decide what type of flag you want to edit:
* If it's a GlobalBitFlag, GlobalByteFlag, GlobalTBoxFlag, TempBitFlag or TempByteFlag, navigate over to `data/res/sys`
* If it's a TempMapBitFlag or TempMapByteFlag, navigate over to `data/res/map/<MAP>/<MAP>.pck`.
  * Meaning, go to `data/res/map`, open the folder associated with your map, and open the `<MAP>.pck`, e.g. for Uptown Springdale in most games it'd be `t101g00.pck`.
* Next, you will find either a `flag_config*.cfg.bin` or `<MAP>_flag.cfg.bin`, depending on the type of flag you are creating. Open it with CfgBin Editor.
  * The `*` refers to versioning, so instead of just a `flag_config.cfg.bin` you might see a `flag_config_0.01.cfg.bin`. Pick the one with the highest version.
* Next, click on the corresponding `FLAG_INFO_*` tree for your type.
  * For `GlobalBitFlag`s and `TempMapBitFlag`s, click on `FLAG_INFO_0`.
  * For `GlobalByteFlag`s and `TempMapByteFlag`s, click on `FLAG_INFO_1`.
  * For `GlobalTBoxFlag`s, click on `FLAG_INFO_2`.
  * For `TempBitFlag`s, click on `FLAG_INFO_3`.
  * For `TempByteFlag`s, click on `FLAG_INFO_4`.
  * Increase the `ChildCount` by 1.
  * Duplicate the last entry in the `FLAG_INFO_*` tree and select the newly created entry.
  * Increment the `FlagSlot` (Increase by 1).
  * Create a new ID for the `FlagID` field.
    * For generic flags, no template is needed, for flags for some specific purposes, a template *IS* needed. Those would be covered in their own specific guides however.
  * Save and Enjoy!
