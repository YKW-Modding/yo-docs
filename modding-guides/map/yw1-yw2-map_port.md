---
title: YW1 -> YW2
layout: default
grand_parent: Map Modding
parent: Map Porting
has_children: false
nav_order: 1
---

# YW1 -> YW2
For Yo-kai Watch 1 to Yo-kai Watch 2, the only change you need to make is NPCs:
* You might need to place npcs in an `npc.pck` for them to register.
* If the YW1 NPC has `OnTalk` scripts, you must convert them into a trigger in the YW2 map’s `.xq` file:
  * You can do this by recompiling the NPC for Yo-kai Watch 2 via NPCMake
* The NPCs `AppearCond` might need adjusting to appropriately match Yo-kai Watch 2, e.g. it might reference a flag that doesn't exist.
This is an *extremely simple* process and definetly the easiest port.
