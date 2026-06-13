---
title: Adding Battles
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---

# How to Add Battles
> Original guide by @kirasnuggets on discord, rewritten by @n123original. This guide assumes you already know how to navigate romfs and use CfgBin Editor. If not, please read [the starting guide](../gettingstarted.html). Additionally, this guide does not explain how to modify extra features such as audio, battle rules, and how to add bosses. 

* First, extract and open `common_enc*.cfg.bin`, from `data/res/battle`.
  * The `*` refers to versioning, so instead of just `common_enc.cfg.bin` you might also see `common_enc_0.03a.cfg.bin`. Pick the one with the highest version.
* Next, duplicate an `ENCOUNT_TABLE_*` entry.
* Then increment (increase by 1) the `ChildCount` of the `ENCOUNT_TABLE` tree that contains the entries.
* Next, do the same for `ENCOUNT_CHARA` for every Yo-kai you want in the battle (1-6 Yo-kai, most encounters only have 3 or less).
  * For each entry, make sure to configure their `ParamID` and `Level`s, to match whatever you want.
* Next, go back to the `ENCOUNT_TABLE_*` entry you created (will be the last one in the tree), and change the `EncountID`.
* Finally, in your entry modify your `EncountCharaOffset*` entries.
  * For instance, if you want the 1st Yo-kai to be `ENCOUNT_CHARA_123` set `EncountCharaOffset1` to `123`.
  * If you don't want to place any Yo-kai in a specified slot, set the corresponding offset to `-1`.
