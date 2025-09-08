---
title: Angle Variables, Cube Glyphs and String Formatting
layout: default
has_children: false
parent: Modding Resources
---
# Angle Variables and Cube Glyphs
## Cube Glyphs
> Note: Even though 3DS fonts support up to the BMP (65k codepoints) - including the standard font: "rodin NTLG" - the custom rasterised in-game fonts do not. The BMP is also quite restrictive not allowing most glyphs or any emojis.
These display icons/glyphs in text, similar to emojis such as play coins! They can be placed in any text cfg.bin:
* `[g_coin]` - Confirmed to work in Yo-kai Watch 2
  * Named after ゲームコイン (Gēmukoin) aka game coin - the Japanese term for Playcoins. Used for the Crank-a-kai.
* `[key]` - Confirmed to work in Yo-kai Watch 2
  * This key glyph is used to signify "Key Quests" in the game - and is used in nearly half the phase text cfg.bins!.
* `[home]` - Confirmed to work in Yo-kai Watch 2
  * This is only used in one text file in the game - the text that appears after you have successfully saved the game.
* `[next_arrow]` - Confirmed to work in Yo-kai Watch 2
* `[misn_arrow]` - Confirmed to work in Yo-kai Watch 2
* `[misn_arrow2]` - Confirmed to work in Yo-kai Watch 2
* `[watch]` - Confirmed to work in Yo-kai Watch 2
* `[mission]` - Confirmed to work in Yo-kai Watch 2
* `[mission2]` - Confirmed to work in Yo-kai Watch 2
* `[btn_l_w]` - Confirmed to work in Yo-kai Watch 2
* `[btn_r_w]` - Confirmed to work in Yo-kai Watch 2
* `[btn_a_w]` - Confirmed to work in Yo-kai Watch 2
* `[btn_x]` - Confirmed to work in Yo-kai Watch 2

## Angle Variables
> **Note: Thank you to @GotaZ for documenting C0 through C30!**

In Yo-kai Watch, you may notice some strings have `<SOMETHING>TEXT</SOMETHING>` or just `<SOMETHING>`. These are called Angle Variables and have a multitude of effects. 

### Colors
> Note: Color Tags are **not** self closing; for obvious reasons you dont just do `<CR>TEXT` you do `<CR>TEXT</C>` so the game knows which portion to color. All other Angle Variables documented here ***are** self-closing.
There are 4 ways to display colors:
* 1. Fixed Alphabetical Color Codes i.e. `<CR>RED</C>`, `<CG>GREEN</C>` (Confirmed to work in all games since Yo-kai Watch 1) and `<CN>IDR</C>` (Confirmed to work in Yo-kai Watch 2 and later).
* 2. RGB888: `<C#RGB888>` codes can be used i.e. for the color `#123456` that would be `<C#123456>`
* 3. RGB776: This is more complicated to visualise but takes the format `<C"RGB776>`. Both RGB888 and RGB776 codes can be generated from the website [YWColor](https://n123git.github.io/yw-color/).
* 4. Fixed Numerical Color Codes there are 31 color codes (0-30) that give a fixed color i.e. `<C3>RED TEXT</C>`:
```xml
<C0>: Fluorescent green
<C1>: Black
<C2>: White
<C3>: Red
<C4>: Yellow
<C5>: Green
<C6>: Blue
<C7>: Beige
<C8>: Orange
<C9>: Beige
<C10>: Gray
<C11>: Gray
<C12>: Pink
<C13>: Light blue
<C14>: Light yellow
<C15>: Black
<C16>: Dark gray
<C17>: Yellow
<C18>: Fluorescent green
<C19>: Pink
<C20>: Beige white
<C21>: White
<C22>: White
<C23>: White
<C24>: Red
<C25>: Blue
<C26>: Green
<C27>: Yellow
<C28>: Black
<C29>: Black
<C30>: Black
```

### Data

#### General
These work in all text `cfg.bins` - even some that reference filenames! (namely for the `<GENDER>` exclusive scenes and such)
* `<NAME>` returns the current player's (ingame) name.
* `<GENDER>` returns the current player's (ingame) gender.
#### Special
These only work in certain `cfg.bins`:
* `<APP_NAME>` - returns the Yo-kai Pad App unlocked.
* `<ITEM_NAME>` - Name of Item.
* `<ITEM_NUM>` - Quantity of Item.
* `<ITEM_NAME1>` - Name of the first item recieved when there are multiple.
* `<ITEM_NAME2>` - Name of the second item recieved when there are multiple.
* `<QUEST_NAME>` - Quest Name
* `<PERSONAL_LOAF>`
* `<PERSONAL_AI>`
* `<YOKAI_NAME>` - Name of yokai
* `<STATION_NAME>` - Name of station.
* `<TRAIN_VOICE>`
* `<ROUTE_NAME>`
* `<MAP_NAME>`
* `<TRAIN_ROUTE>`
* `<TRAIN_TYPE>`
* `<POSSESS_NUM>` - Amount Possessed i.e. Gate Globes
* `<INSERT_NUM>`
* `<ROM>`
* `<PNAME>`

## String Formatting
Strings are multi-line bodies of shift-jis/utf8/cp932 text depending on the `cfg.bin`.

They support control characters using the ASCII LF (`\n` in CfgBin Editor) for new-lines. As shown above they support Angle Variables and Cube Glyphs for glyphs and dynamic insertion. However they also support basic C formatting:
* `%d` - Integer Placeholder (Used in Play-Coin Remaining text for Yo-kai Watch 2)
* `%s` - String Placeholder
* `%f` - Float Placeholder
* `%x` - Hex Int Placeholder (Most likely never used in public releases)
* `%c` - Single Char Placeholder

etc, for brevity reasons only a few are listed above.
# TEMP NOTES; YOU SHOULD NOT SEE THIS
* `<GZ><SH><X120><Y18>[mj_o][mj_h][mj_h][mj_h][mk_em][mk_em]` - what is this..........
  * `<GZ><SH><X23><Y18>[mj_i][mk_sp][mj_j][mj_u][mj_s][mj_t][mk_sp][mj_r][mj_e][mj_m][mj_e][mj_m][mj_b][mj_e][mj_r][mj_e][mj_d][mk_em][mk_em]` - this too...............
    * `<SH><X100><Y18>[mj_n][mj_o][mj_o][mj_o][mj_o][mj_o][mk_em][mk_em]` - ok I wont even bother documenting these............. 

* `<SEL0005/1>` - ??? (`system_text_engb.cfg.bin` - Yo-kai Watch 2)
* `<SEL0005/2>` - ??? (`system_text_engb.cfg.bin` - Yo-kai Watch 2)
* `<SEL0024/1>` - Eyepo/Medallium/Mirapo/StampRally related?? (`system_text_engb.cfg.bin` - Yo-kai Watch 2)
* `<SEL0001/1/2>` - ??? (`system_text_engb.cfg.bin` - Yo-kai Watch 2)
* `<SEL0003/1/3>` - ???
* `<O34>Crank the Crank-a-kai?<SEL0003/2/3>`
* `<O34>` - ??? Watch Rank Gate Dialogue
* `<QVAL#qr0011/#ruby_hiki>` - ???
* `<QVAL#qr0012/#ruby_hiki>` - ???
* `<J"\"J_07\"\">` - ???
* `<O34>Get <VAL#tmp_seek_num> within <VALMMSS#tmp_quest_time> min.`
* `Win <VAL#tmp_gate_rest_enemy> battle(s)!`
* `<V"tr0401">`
* `<Y10>"<CG><QUEST_NAME></C>" started!`
