---
title: Making Custom Events Work Properly
layout: default
parent: Creating Your Own Postgame Stories
grand_parent: Modding Guides
---

# Making Custom Events Work Properly
**Original author on Discord: @z_u_ra**


Sometimes, when you add an event XQ and try to do stuff like spawning models or starting dialogue, it just won't work! Here's how to fix it.

1. Open `data/res/sys/event_set_config_0.01.cfg.bin`
2. Increment the value of the first entry (child count)
3. Duplicate a child of the first entry
4. Activate Yw2 tags
5. in the EventID field, put your hashed event ID. You can hash your event ID here: https://emn178.github.io/online-tools/crc32.html
6. Change all the documented variables to your liking and leave all of the undocumented variables as is.
7. Enjoy.
