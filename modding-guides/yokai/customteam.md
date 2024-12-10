---
title: How to Make a Battle with a Custom Team
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---
# How to Make a Battle with a Custom Team
**Original guide by @pqr29 on Discord**

## 0. Dump your romfs with Citra


## 1. Tools that you need

-Kuriimu2 https://github.com/FanTranslatorsInternational/Kuriimu2

-CfgBinEditor 
https://github.com/Tiniifan/CfgBinEditor

-The MyTags.txt from <#1146912428748177449> 

## 2. Create a BattleRule

First extract the `battle_rule_config_0.03g.cfg.bin` from `yw2_a.fa/data/res/battle`,

Then open it with cfgbineditor and clone `BTL_RULE_INFO_2` check the variable 0 as hex and put a random value on it

## 3. Set your custom team

First extract the `common_enc_0.03a.cfg.bin` from `yw2_a.fa/data/res/battle`  and create 6 entries in `ENCOUNT_CHARA` (the yokais for the custom team) 

Then go to `ENCOUNT_TABLE` and clone `ENCOUNT_TABLE_414`
Change the id and put a random one then change the EncountCharaOffsets with the offset of your custom team

Next go to your battle rule config, open it go to the 4 sub entry and change the variable 8 and put the id of your `ENCOUNT_TABLE` entry.

Finally go to the encounter that you want to have custom team and put the variable 0 of your battle rule config in `BattleRuleID`, enjoy your battle with custom team

