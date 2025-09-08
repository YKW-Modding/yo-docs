---
title: Angle Variables and Cube Glyphs
layout: default
has_children: false
parent: Modding Resources
---
# Angle Variables
> **Note: Thank you to @GotaZ for documenting C0 through C30!**

In Yo-kai Watch, you may notice some strings have `<SOMETHING>TEXT</SOMETHING>` or just `<SOMETHING>`. These are called Angle Variables and have a multitude of effects.

## Colors

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

## Data

### General
* `<NAME>` returns the current player's (ingame) name.
* `<GENDER>` returns the current player's (ingame) gender.
### Special
These only work in certain `cfg.bins`:
* `<APP_NAME>` - returns the Yo-kai Pad App unlocked.
* `<SEL0005/1>` - ??? (`system_text_engb.cfg.bin` - Yo-kai Watch 2)
* `<SEL0005/2>` - ??? (`system_text_engb.cfg.bin` - Yo-kai Watch 2)
* `<SEL0024/1>` - Eyepo/Medallium/Mirapo/StampRally related?? (`system_text_engb.cfg.bin` - Yo-kai Watch 2)
* `<SEL0001/1/2>` - ??? (`system_text_engb.cfg.bin` - Yo-kai Watch 2)
* `<SEL0003/1/3>` - ???
* `<O34>Crank the Crank-a-kai?<SEL0003/2/3>`
* `<PERSONAL_LOAF>`
* `<PERSONAL_AI>`
* `<ITEM_NAME>` - Name of Item.
* `<ITEM_NUM>` - Quantity of Item.
* `<ITEM_NAME1>` - Name of the first item recieved when there are multiple.
* `<ITEM_NAME2>` - Name of the second item recieved when there are multiple.
* `<YOKAI_NAME>` - Name of yokai
* `<STATION_NAME>` - Name of station.
* `<O34>` - ??? Watch Rank Gate Dialogue
* `<QVAL#qr0011/#ruby_hiki>` - ???
* `<QVAL#qr0012/#ruby_hiki>` - ???
* `<J"\"J_07\"\">` - ???
* `<QUEST_NAME>` - Quest Name
* `%d`
* `<O34>Get <VAL#tmp_seek_num> within <VALMMSS#tmp_quest_time> min.`
* `<POSSESS_NUM>` - Amount of something possessed i.e. Gate Globes
* `<INSERT_NUM>`
* `Win <VAL#tmp_gate_rest_enemy> battle(s)!`
* `<TRAIN_VOICE>`
* `<ROUTE_NAME>`
* `<MAP_NAME>`
* `<V"tr0401">`
* `<TRAIN_ROUTE>`
* `<TRAIN_TYPE>`
* `<Y10>"<CG><QUEST_NAME></C>" started!`
* `<ROM>`
* `<PNAME>`
# Cube Glyphs
These display icons/glyphs in text, similar to emojis such as play coins! They can be placed in any text cfg.bin
* `[g_coin]` - Confirmed to work in Yo-kai Watch 2
  * Named after ゲームコイン (Gēmukoin) aka game coin - the Japanese term for Playcoins.
* `[key]` - Confirmed to work in Yo-kai Watch 2
* `[next_arrow]` - Confirmed to work in Yo-kai Watch 2
* `[watch]` - Confirmed to work in Yo-kai Watch 2
* `[mission]` - Confirmed to work in Yo-kai Watch 2
* `[mission2]` - Confirmed to work in Yo-kai Watch 2
* `[btn_l_w]` - Confirmed to work in Yo-kai Watch 2
* `[btn_r_w]` - Confirmed to work in Yo-kai Watch 2
* `[misn_arrow]` - Confirmed to work in Yo-kai Watch 2
