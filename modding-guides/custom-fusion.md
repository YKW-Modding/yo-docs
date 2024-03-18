---
title: How to make a custom fusion ?
layout: default
parent: Modding Guides
---
**Original guide by @whisperito on discord**

*This guide works with most 3DS games but B1 and B2*

__**Step 0 : Getting tools :**__

You'll need :

[Kuriimu2](https://github.com/FanTranslatorsInternational/Kuriimu2/releases/latest)
[CfgBinEditor (Tiniifan)](https://github.com/Tiniifan/CfgBinEditor/releases/latest) 
or 
[CfgBinEditor (OnePieceFreak)](https://github.com/OnePieceFreak3/CfgBinEditor/releases/latest)
(both work)

*Not __necessaries__ but recommendeds :* [The latest MyTags](https://discord.com/channels/1053460697754898432/1207616278051946516)

__**Step 1 : Extracting necessary files**__

1. Open your main fa `yw[?]_a.fa` (there may not be a caracter instead of ?) using Kuriimu2
2. Go to `data/res/shop` and extract `combine_config.cfg.bin` (you should also need to extract `chara_param[???].cfg.bin` in `data/res/character`(always take the heaviest one if there are multiples ones)

__**Step 2 : Editing the files **__

*0. If you chose to use the tags, please make sure that you've put the downloaded tags into the same folder as the CfgBinEditor executable*
*1. First, make sure your Yo-Kai is fusable, for this, open the `chara_param[???].cfg.bin` using your CfgBinEditor, choose the tags for your game and go to the CHARA_PARAM entry of your yo-kai, scroll down and look for an `IsFusable` value (or if you're not using the tags, it's one of the last values generally, it's a boolean), make sure this value is `1`.*
2. Open `combine_config.cfg.bin` using your CfgBinEditor, choose the tags for your game.
3. Duplicate an entry in `COMBINE_CONFIG_BEGIN_0` (`_BEGIN_0` may or may not happen depending of your CfgBinEditor) and generate an ID on [this site](https://emn178.github.io/online-tools/crc32.html), put the generated ID in the `FusionID` variable (make sure it's as `HEX`)
4. If the base of the fusion (one of the two elements of the fusion) is an item, put `1` in the `BaseIsItem`, if it's a yo-kai, put `0`, then, in BaseID, put the ID of the first element (either way your `ItemID` or the Yo-Kai's `ParamID`)

5. If the material (second of the two elements of the fusion) is an item, put `1` in the `MaterialIsItem` value, if it's a yo-kai, put `0`, then, in `MaterialID` put the ID of the second element (either way your ItemID or the Yo-Kai's ParamID)
6. Then, if the result of your fusion is an item, in the `EvolveToIsItem` value, put `1`, if it's a Yo-Kai, put `0`, next, in the `EvolveToID` value, put your result's ID (either way your ItemID or the Yo-Kai's ParamID)
7. I actually don't know what's `FusionIsItem`, I'll update the guide if I figure out!
8. Click on your entry list, (in this case it's `COMBINE_CONFIG_BEGIN_0`) (`_BEGIN_0` may or may not happen depending on your CfgBinEditor) and increase the value you see at the right by the number of entries you added! Then, you can save without any problems your `.cfg.bin`

__**Step 3 : Saving, and, testing.**__

1. Replace the files you edited using Kuriimu2
2. Save your `.fa`
3. [Test the mod!](https://discord.com/channels/1053460697754898432/1146817769044717668)
