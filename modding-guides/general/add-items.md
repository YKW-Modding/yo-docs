---
title: Adding Items
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# How to Add Items
**Original guide by @stringsbutalt on discord, rewritten, and improved by @n123original**

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

7. Remember your Name TextID & Description TextID. you'll need em 4 later


## **Making the Item Icon**
1. Open ``data/menu/item_icon.xi`` in K2

2. Open the png, extract the png. Edit the png to add a new item to the next slot in the array (image below to make it easier for yw1)

3. Copy a item like item_002.xi for example

4. rename it to the next available item number in my case 309. (this'll be your Offset2 in item step)

5. Change the texture to your custom items texture


## **Registering the Item**

1. Open `data/res/item/item_config_0.05d.cfg.bin` in CfgBinEditor

2. On the entry group of your item type add 1 to the value. If your making a consumable the entry group will be `ITEM_CONSUME`, if your making equipment it'll be `ITEM_EQUIPMENT`.

3. Open your entry group.

4. Duplicate one of the entries.

5. Set your tags to `YW1`

6. Generate a ItemID from a crc website https://emn178.github.io/online-tools/crc/. Put anything you'd like it should turn out smth like 0x50ED2AAD. Now add that as your ItemID

7. Add The NameID we made in the previous step.

8. Add The Inventory Sort. this is the category your item will go in.
Yo-kai Watch 1 `InventorySort`(s):
* `10`: Food
* `20`: Misc Items
* `30`: Critters  (Bugs & Fish)
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

13. Set your CarryCap (Max amount of items you can have in a stack. Usually is 99)

14. Set your SellPrice & PurchasePrice. 

15. Set `CanBeBought`, `CanBeSold` and `IsFusable`:
* 0 - No (False)
* 1 - Yes (True)

16. Set the offsets for `ItemPosX` & `ItemPosY`. It should be how far across is the item and how far down in `item_icon.xi` (note: it's zero indexed meaning row 0 is the 1st, row 1 is the 2nd etc; same for columns). Find this based on what we did on the last step.

16. Set your `InvMenuTextID` and `NounTextID` to the `TextID` you made for your item in the previous step.

17. Open `ItemIndex` and add 1 to the entry group value (`ChildCount` of the tree).

18. Duplicate a entry. Set the `ItemID` to the `ItemID` you created. Set the `InventorySort` to your `InventorySort`, and the `MainItemIndex` to the last entry's `MainItemIndex` + 1.

If it's an equipment: 
* Set `STRBuff` and the Buff's for all the other stat's to the stat changes you want i.e. 5 = +5, -5 = -5, 0 = unchanged.
* Optionally, set `SkillIDA` (and `SkillIDB` if you need two) to the `SkillID`s for the special effect you want your equipment to have.
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
**BASE ITEM IMAGE:**

![Base item image](yw1_mainitemicon.png) <!-- the original version was on imgur 😭 -->
