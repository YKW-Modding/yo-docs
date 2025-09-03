---
title: Conds
layout: default
has_children: false
parent: Modding Resources
---

This page is a list of Conds (Conditions) and the game they were discovered (Most conds usually work between games; so using others *should be fine*):
* Note: Some conds are customisable in that case the raw bytes will be linked, use a website like [this](a) to convert it to base64.
* `0` - Always True. (Global)
* `AAAAAA8FNRCxQJYAAQAyAAAAAXg=` - Main Story Completion (Yo-kai Watch 2). Obtained from Jungle Hunter (`shop_shopT001_0.01n.cfg.bin`)
* `00 00 00 00 0f XX 35 10 b1 40 96 00 01 00 32 00 00 00 01 78` - Rank `XX` or higher Cond (Yo-kai Watch 2; replace XX with the rank, `00` = E, ... `05` = S). Obtained from `Jungle Hunter` (`shop_shopT001_0.01n.cfg.bin`).
* `00 00 00 00 18 05 35 8d 76 66 d8 00 0a 01 28 00 06 02 XX XX XX XX 00 32 00 00 00 01 78` - Has Item Cond. (Obtained from @z.u.ra on discord; replace `XX XX XX XX` with the ItemID in hex - remember to place the bytes from right to left).
  * `Level5Condition.exe` can also generate this Cond.
* `1` - Used in place of a Cond in certain situations. (Yo-kai Watch 2)
