---
title: Conds
layout: default
has_children: false
parent: Modding Resources
---

This page is a list of Conds (Conditions) and the game they were discovered along with format documentation (*Yo-kai Watch 2 Conds DO NOT always work in other games i.e. Yo-kai Watch 3 - although they may. Please test this yourself*).
Conds are structured, binary conditional data (stored as base64 strings) which control conditions for everything from NPCs to Quests and even what items are available in shops!
* Note: ***Some conds are customisable, in that case the raw bytes will be linked, use a website like [this](https://cryptii.com/pipes/hex-to-base64) to convert it to base64.***
* `0` - Always True / Null Cond. (Global)
  * `1` - Used in place of a Cond in certain situations. (Yo-kai Watch 2)
* `AAAAAA8FNRCxQJYAAQAyAAAAAXg=` - Main Story Completion (Yo-kai Watch 2). Obtained from Jungle Hunter (`shop_shopT001_0.01n.cfg.bin`)
* `00 00 00 00 0f 05 35 dd 77 26 95 00 01 00 32 00 00 00 XX 71` - Rank XX or higher Cond (Yo-kai Watch 2; replace XX with the rank, 00 = E, ... 05 = S). Obtained from Jungle Hunter (`shop_shopT001_0.01n.cfg.bin`).
* `00 00 00 00 18 05 35 8d 76 66 d8 00 0a 01 28 00 06 02 XX XX XX XX 00 32 00 00 00 01 78` - Has Item Cond. (Obtained from @z.u.ra on discord; replace XX XX XX XX with the ItemID in hex - remember to place the bytes from right to left).
  * Level5Condition.exe can also generate this Cond.
