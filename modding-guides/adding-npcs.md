---
title: Adding NPCs
layout: default
parent: Modding Guides
---

# Adding NPCs to a map
**Original author on Discord: @komazuraaa**


## Required tools
CfgBinEditor

Kuriimu2

ArchiveL5
## The Guide

Difficulty: Advanced. This guide assumes knowledge with using CfgBinEditor and adding entries.

Make sure you are using the latest CfgBin tags.

1. Open your resmap folder in your main FA. The path to it is data/res/map/[your_map_id]
2. Open your npc_set cfgbin. the filename should be [map_id]_npc_set_0.01b.cfg.bin
3. Add an entry in NPC_BASE. Set the NPC ID to an ID of your choice. You can generate one with any CRC32 calculator. BaseID should be the BaseID of your NPC. NPCType should be 2 for human. I don't remember the other types right now. Everything else should be 0.
4. Add an entry in NPC_PRESET. NPCID should be the same ID as before, NPCAppearCount should be 1, and we'll fill the NPCAppearStartOffset later.
5. Add an entry in NPC_APPEAR. NPCName should be the NPCName of your choice.  PhaseAppear should be 0. Leave the rest as is.
6. Back in your NPC_PRESET entry, fill the NPCAppearStartOffset with the offset of your NPC_APPEAR entry.
7. Put the CfgBin back in the game.
8. Extract the npc.pck file and open it in ArchiveL5.
9. From the npc.pck, take a random npcbin and extract it. Rename it to the NPCName we set earlier, and open it in CfgBinEditor. Go to the POINT entry. the first value there should be a float. This is the format of the float:
`x.y`
that means that the number before the period is the X position of the NPC, and the number after the dot is the Y position of the NPC.
10. Now, add the NPCBIN to your npc.pck. Save your npc.pck and put it in the game.
11. Now your NPC is in the game!!!! Enjoy. You can check out some other guides or ask around to know how to do stuff with the NPCs.
