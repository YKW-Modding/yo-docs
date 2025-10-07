---
title: Conds (Conditions)
layout: default
has_children: false
parent: Modding Resources
---
This page is a list of Conds (Conditions), along with where they are found (*Yo-kai Watch Conds DO NOT always work across games i.e. Yo-kai Watch 3 - although they may. Please test this yourself*).

# Documented Useful Conds List
* Note: ***Some conds are customisable, in that case the raw bytes will be linked, use a website like [this](https://cryptii.com/pipes/hex-to-base64) to convert it to base64.***
* `0` - Always True / Null Cond. (Global)
  * `1` - Used in place of a Cond in certain situations. (Yo-kai Watch 2)
* `AAAAAA8FNRCxQJYAAQAyAAAAAXg=` - Main Story Completion (Yo-kai Watch 2). Obtained from Jungle Hunter (`shop_shopT001_0.01n.cfg.bin`)
* `00 00 00 00 0f 05 35 dd 77 26 95 00 01 00 32 00 00 00 XX 71` - Rank XX or higher Cond (Yo-kai Watch 2; replace XX with the rank, 00 = E, ... 05 = S). Obtained from Jungle Hunter (`shop_shopT001_0.01n.cfg.bin`).
* `00 00 00 00 18 05 35 8d 76 66 d8 00 0a 01 28 00 06 02 34 XX XX XX XX 32 00 00 00 01 78` - Has Item Cond. (Obtained from @z.u.ra on discord; replace XX XX XX XX with the ItemID in hex - remember to place the bytes from right to left).
  * Level5Condition.exe can also generate this Cond.
* `00 00 00 00 18 05 35 9e 99 84 8c 00 0a 01 28 00 06 02 34 89 20 ff e7 32 00 00 00 XX 78` - Has beaten the tunnel XX times (Yo-kai Watch 2; Obtained from `enen_tunnel_event.cfg.bin`.

<!--
## Examples:

* Basic Conds:
  * Watch Rank >= XX (YW2) Cond:
     * `00 00 00 00 0f 05 35 dd 77 26 95 00 01 00 32 00 00 00 05 71`
     * `00 00 00 00` - Header.
     * `0f 05` - Opcode; most likely `constant >=`.
     * `35` - Section Delimiter.
     * `DD 77 26 95` - Resource ID.
     * `00 01 00` - Ctype (Basic).
     * `32` - Section End Delimiter.
     * `00 00 00 05` - Comparison Value in this case `0x00000005` (S) for Watch Rank S.
     * `71` - Basic Terminator.
  * Main Story Completed (YW2) Cond:
     * `00 00 00 00 0f 05 35 10 b1 40 96 00 01 00 32 00 00 00 01 78`
     * `00 00 00 00` - Header.
     * `0f 05` - Opcode; most likely `constant >=`.
     * `35` - Section Delimiter.
     * `10 b1 40 96` - Resource ID.
     * `00 01 00` - Ctype (Basic).
     * `32` - Section End Delimiter.
     * `00 00 00 01` - Comparison Value in this case `0x00000001` (1) for `true`.
     * `78` - Basic Terminator.

* Advanced (State Conds):
  * Unknown Flag Cond (YW4):
    * `00 00 00 00 18 05 35 2a 3d 45 43 00 0a 01 28 00 06 02 34 2c b8 6a 25 32 00 00 00 01 78`
    * `00 00 00 00` - Header.
    * `18 05` - Opcode; Seems to be flag comparison.
    * `35` - Section Delimiter.
    * `2a 3d 45 43` - Resource ID A.
    * `00 0a 01` - Ctype (This Ctype is for Extended Format).
    * `28` - Extension Delimiter.
    * `00 06 02` - Ctype B.
    * `34` - Nest Delimiter??
    * `2c b8 6a 25` - Ressource ID B.
    * `32` - Section End Delimiter.
    * `00 00 00 01` - Comparison Value (`0x00000001` aka in this case `true`; remember types are implicit).
    * `78` - `0x78` Terminator.
  * Has Beating the Tunnel XX times (YW2):
    * `00 00 00 00 18 05 35 9e 99 84 8c 00 0a 01 28 00 06 02 34 89 20 ff e7 32 00 00 00 XX 78`
    * `00 00 00 00` - Header.
    * `18 05` - Opcode; Seems to be flag comparison.
    * `35` - Section Delimiter.
    * `9e 99 84 8c` - Resource ID A.
    * `00 0a 01` - Ctype (This Ctype is for Extended Format).
    * `28` - Extension Delimiter.
    * `00 06 02` - Ctype B.
    * `34` - Nest Delimiter??
    * `89 20 ff e7` - Ressource ID B.
    * `32` - Section End Delimiter.
    * `00 00 00 XX` - Comparison Value (`0x000000XX`).
    * `78` - `0x78` Terminator.

-->
