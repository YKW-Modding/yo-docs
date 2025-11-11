---
title: Adding Items
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# How to Add Items
**Original guide by @stringsbutalt on discord. Rewritten, expanded on and improved by @n123original**

**CONFIRMED TO WORK ON YO-KAI WATCH 1 & 2.**

# Yo-kai Watch 1

## **Making the Item Name & Description**
1. Open `data/res/text/item_text_<language>.cfg.bin`; where `<language>` is the language you want to make the name for. For example:
* `ja` for Japanese
* `fr` for French
* `en` for American English
* `engb` for European English
* `it` for Italian, etc

3. Add 2 to the value of entry group `TEXT_INFO`

4. Duplicate 2 entries.

5. In the duplicated entries generate a new CRC. Set it as the TextID. You should have 2 now. One for the Item Name & Item Description.

6. Now for your name entry set the "Text" field to your items name. In description entry set the "Text" to your items description.

7. Remember your Name TextID & Description TextID. you'll need these for later


## **Making the Item Icon**
1. Open ``data/menu/item_icon.xi`` in K2

2. Open the png, extract the png. Edit the png to add a new item to the next slot in the array (image below to make it easier for yw1)

3. Copy a item like item_002.xi for example

4. rename it to the next available item number in my case 309. (this'll be your Offset2 in item step)

5. Change the texture to your custom items texture


## **Registering the Item**

1. Open `data/res/item/item_config_0.05d.cfg.bin` in CfgBinEditor

2. On the entry root entry of your Item's category and increase the `ChildCount` by 1.
  * If your making a consumable (Food and misc. Items) the entry group will be `ITEM_CONSUME`, if your making Equipment it'll be `ITEM_EQUIPMENT`, Key Items are under `ITEM_IMPORTANT` and Critters (Bugs/Fish/Insects) are under `ITEM_CREATURE`.

4. Open your root entry.

5. Duplicate one of the child entries.

6. Set your tags to `YW1`

7. Generate a ItemID from a CRC-32 website such as [this](https://emn178.github.io/online-tools/crc/)
* Tyoe anything you'd like it should turn out as something like `0x50ED2AAD` (this example website dosen't show the `0x` but add it anyway). Now add that as your `ItemID`

9. Add The `NounTextID` we made in the previous step.

10. Add The `InventorySort`. this (and the item category i.e. `ITEM_CONSUME`) determine the category your item will be sorted in within the game's UI.

Yo-kai Watch 1 `InventorySort`(s):
* `10`: Food
* `20`: Misc Items
* `30`: Critters (Bugs & Fish)
* `40`: Equipment
* `50`: Key Items

9. Add the ItemType:

Yo-kai Watch 1 `ItemType`(s):
* `0`: Misc Items
* `1`: Rice Balls
* `2`: Bread
* `3`: Candy
* `4`: Milk
* `5`: Juice
* `6`: Burgers
* `8`: Ramen
* `10`: Chinese Food
* `12`: Vegetables
* `13`: Meat
* `14`: Fish (Food)
* `20`: Fish (Critters)
* `21`: Bugs/Insects
* `30`: Equipment
* `40`: Key Items
* `41`: Trophies
* `50`: Talismans
* `51`: Medicine
* `52`: Getaway Plush
* `53`: Staminums
* `54`: Exporb
* `55`: Move level Books
* `56`: Attitude Books
* `57`: Loaf Books
* `70`: Crank-a-kai Coins

11. Set your Offsets.
 * Set `ItemNum` to the the number of the previous item + 1
 * Set `GlobalItemIndex` to the number of your item xi. So item_309.xi would be 309.

13. Set your `CarryCap` (Maximum amount of items you can have in a stack. This is usually 99)

14. Set your `SellPrice` and `DefaultBuyPrice`. 

15. Set `CanBeBought`, `CanBeSold` and `IsFusable`:
* `0` - No (False)
* `1` - Yes (True)

16. Set the offsets for `ItemPosX` and `ItemPosY`.
* It should be how far across is the item and how far down in `item_icon.xi`
> Note: This is *zero-indexed* meaning an `IconPosY` of `0` refers to the first row, `1` refers to the second row etc; same for columns aka `IconPosX`.

16. Set your `InvMenuTextID` and `NounTextID` to the `TextID` you made for your item in the previous step.

17. Open `ItemIndex` and add 1 to the entry group value (`ChildCount` of the tree).

18. Duplicate a entry. Set the `ItemID` to the `ItemID` you created. Set the `InventorySort` to your `InventorySort`, and the `MainItemIndex` to the last entry's `MainItemIndex` + 1.


If it's a consumable:
* Set `EffectA` to the effect type and `EffectAValue` to it's value. Same for `EffectB` if you need a 2nd effect. I.e. set `EffectA` to `Infinite Stamina (Seconds)` and `EffectAValue` to 30:

Yo-kai Watch 1 Consumable `Effect`(s):
* `0` - N/A  (No Effect)
* `1` - Run BtlCommandID (Used for special effects such as feeding enemies for befriending)
* `2` - Heal HP
* `3` - Raise XP
* `4` - Raise Attack/Technique/Soultimate level by 1,  3 increases Soultimate Level, 2 increases Technique Level and  1 increases Attack Level.
* `5` - Edits a Yo-kais Move Attitude, NOT loaf attitude.
* `6` - Infinite Stamina (Seconds)
* `7` - Raise Attitude Points (used for EV developement; aka stat buffs)

If it's an equipment: 
* Set `STRBuff` and the Buff's for all the other stat's to the stat changes you want i.e. 5 = +5, -5 = -5, 0 = unchanged.
* Optionally, set `SkillIDA` (and `SkillIDB` if you need two) to the `SkillID`s for the special effect you want your equipment to have.
Finally, if you don't want your equipment to have any requirements set `EquipRequirement` to 0, otherwise follow the steps below:

### Equipment Requirements

If the game has already registered a requirement you want to use for an item use one of the following, otherwise continue to create custom requirements:

Yo-kai Watch 1 Vanilla Equip Requirements:
* `0` - N/A
* `1` - D-rank or lower Yo-kai
* `2` - Cicada Yo-kai (Cadin, Cadable and Singcada)
* `3` - Cat Yo-kai (Jibanyan, Thornyan, Baddinyan, Robonyan, Goldenyan, Dianyan, Sapphinyan, Emenyan, Rubinyan, Topanyan, Shogunyan)
* `4` - Kappa Yo-kai (Walkappa, Appak and Supyo)
* `5` - Tengu Yo-kai (Tengu and Flengu)
* `6` - Wiglin, Steppa and Rhyth
* `7` - Badude and Bruff
If none of these suit your goals; it's time to make some custom requirements!
* Click on the top-level tree `ITEM_EQUIP_COND` and increase `ChildCount` by 1
* Then duplicate an `ITEM_EQUIP_COND`, if you want your equipment to have a Maximum Rank the yo-kai can be set the `MaximumRank` to that rank:

YW1 Ranks:
* `0`: E
* `1`: D
* `2`: C
* `3`: B
* `4`: A
* `5`: S
 
* If you want other conditions in Yo-kai Watch 1 you unfortunately have to whitelist specific yokai
  * If you **DONT** want to do this set `StartBoundary` and `BoundaryLength` to 0.
* To do this set `StartBoundary` to the amount of `ITEM_EQUIP_COND_CHARA`(s) for example if there's 10 the last one will be called `ITEM_EQUIP_COND_CHARA_9` in CfgBin Editor.
* Then set `BoundaryLength` to the amount of Yo-kai you want to whitelist.
* Then duplicate `BoundaryLength` `ITEM_EQUIP_COND_CHARA`(s) (and increase `ChildCount` of the main/root entry `BoundaryLength` times) and for each `ITEM_EQUIP_COND_CHARA` you've duplicated, set the `BaseID` to the `BaseID` of a Yo-kai you want to whitelist.
**BASE ITEM IMAGE:**

![Base item image](yw1_mainitemicon.png) <!-- the original version was on imgur 😭 -->
