---
title: Adding Flags
layout: default
parent: Creating Your Own Postgame Stories
grand_parent: Modding Guides
---

# Adding Flags
> **Original author on Discord: @z_u_ra, modified by @n123original**

### **NOTE: IN YOKAI WATCH 2, THIS CRASHES THE GAME DUE TO A BUG WITHIN THE ENGINE! Only use this guide in YW1, YW3, YWB1 OR YWB2**

There are several types of flags with the most common type being `GlobalBitFlags`; these are *boolean* meaning they can be either a `1` (enabled/true) or a `0` (disabled/false); you can get/set these in XQ via `get_global_bitflag()` and `set_global_bitflag()`. Another common type is `GlobalByteFlags` which are *8-bit integers* meaning they can be any number between 0 and 255.

## Creating GlobalBitFlags
* Navigate over to `data/res/sys/`
* There you will find a `flag_config.cfg.bin` - open it.
  * If you see multiple, pick the one with the largest number i.e. if theres `flag_config.cfg.bin` and `flag_config_0.01a.cfg.bin`, pick the second option.
* Click on `FLAG_INFO_0`.
  * Increase the `ChildCount` by 1.
* Duplicate the last entry in `FLAG_INFO_0` and select the newly created entry.
* Increase the `FlagSlot` by 1.
* Create a new ID for the `FlagID` field, you can do this by typing anything into a website like [this](https://emn178.github.io/online-tools/crc/).
* Save and Enjoy!

## Creating GlobalByteFlags
* Navigate over to `data/res/sys/`
* There you will find a `flag_config.cfg.bin` - open it.
  * If you see multiple, pick the one with the largest number i.e. if theres `flag_config.cfg.bin` and `flag_config_0.01a.cfg.bin`, pick the second option.
* Click on `FLAG_INFO_1`.
  * Increase the `ChildCount` by 1.
* Duplicate the last entry in `FLAG_INFO_1` and select the newly created entry.
* Increase the `FlagSlot` by 1.
* Create a new ID for the `FlagID` field, you can do this by typing anything into a website like [this](https://emn178.github.io/online-tools/crc/).
* Save and Enjoy!

## Creating GlobalTBoxFlags
* Navigate over to `data/res/sys/`
* There you will find a `flag_config.cfg.bin` - open it.
  * If you see multiple, pick the one with the largest number i.e. if theres `flag_config.cfg.bin` and `flag_config_0.01a.cfg.bin`, pick the second option.
* Click on `FLAG_INFO_2`.
  * Increase the `ChildCount` by 1.
* Duplicate the last entry in `FLAG_INFO_2` and select the newly created entry.
* Increase the `FlagSlot` by 1.
* Create a new ID for the `FlagID` field, you can do this by typing anything into a website like [this](https://emn178.github.io/online-tools/crc/).
* Save and Enjoy!

## Creating TempBitFlags
* Navigate over to `data/res/sys/`
* There you will find a `flag_config.cfg.bin` - open it.
  * If you see multiple, pick the one with the largest number i.e. if theres `flag_config.cfg.bin` and `flag_config_0.01a.cfg.bin`, pick the second option.
* Click on `FLAG_INFO_3`.
  * Increase the `ChildCount` by 1.
* Duplicate the last entry in `FLAG_INFO_3` and select the newly created entry.
* Increase the `FlagSlot` by 1.
* Create a new ID for the `FlagID` field, you can do this by typing anything into a website like [this](https://emn178.github.io/online-tools/crc/).
* Save and Enjoy!

## Creating TempMapBitFlags
* Navigate over to `data/res/map/` and open `<mapName>.pck`.
* Open `<mapName>_flag.cfg.bin`
  * If it is not there; the map has no TempMapBitFlags and you must copy the `cfg.bin` from another map.
* Click on `FLAG_INFO_0`.
  * Increase the `ChildCount` by 1.
* Duplicate the last entry in `FLAG_INFO_0` and select the newly created entry.
* Increase the `FlagSlot` by 1.
* Create a new ID for the `FlagID` field, you can do this by typing anything into a website like [this](https://emn178.github.io/online-tools/crc/).
* Save and Enjoy!
* Create a new ID for the `FlagID` field, you can do this by typing anything into a website like [this](https://emn178.github.io/online-tools/crc/).
* Save and Enjoy!
