---
title: Adding flags
layout: default
parent: Create your own post game stories
grand_parent: Modding Guides
---

# Adding flags
**Original author on Discord: @z_u_ra**


**NOTE: IN YOKAI WATCH 2, THIS CRASHES THE GAME DUE TO A BUG WITH THE ENGINE! ONLY USE THIS IN 1, 3, B1 OR B2**
1. Open `data/res/sys/flag_config_0.01.cfg.bin`
2. Increment the value in FLAG_INFO_0 (child count)
3. Duplicate a child in FLAG_INFO_0
4. Activate YW2 flags
5. Increment the `FlagSlot` by 1.
6. Put any ID you'd like in the `FlagID` field.
7. Enjoy.