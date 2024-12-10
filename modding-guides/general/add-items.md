---
title: Adding Items
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# How to Add Items
**Original guide by @stringsbutalt on discord**

**CONFIRMED TO WORK ON YO-KAI WATCH 1.**

There are 2 types of items you can add first is equipment & consumables. The guide will vary a tad bit for all of them

**Making the Item Name & Description**
1. Open `data/res/text/item_text_ja.cfg.bin`
(Set )

2. Add 2 to the value of entry group `TEXT_INFO`

3. Duplicate 2 entries.

4. In the duplicated entries generate a new CRC. Set it as the TextID. You should have 2 now. One for the Item Name & Item Description.

5. Now for your name entry set the "Text" field to your items name. In description entry set the "Text" to your items description.

6. Remember your Name TextID & Description TextID. you'll need em 4 later


**Making the Item Icon**
1. Open ``data/menu/item/item_icon.xi`` in k2

2. Open the png, extract the png. Edit the png to add a new item to the next slot in the array (image below to make it easier for yw1)

3. Copy a item like item_002.xi for example

4. rename it to the next available item number in my case 309. (this'll be your Offset2 in item step)

5. Change the texture to your custom items texture


**Adding the Item**

1. Open `data/res/item/item_config_0.05d.cfg.bin` in CfgBinEditor

2. On the entry group of your item type add 1 to the value. If your making a consumable the entry group will be `ITEM_CONSUME`, if your making equipment it'll be `ITEM_EQUIPMENT`.

3. Open your entry group.

4. Duplicate one of the entries.

5. Set your tags to `YW1`

6. Generate a ItemID from a crc website https://emn178.github.io/online-tools/crc/. Put anything you'd like it should turn out smth like 0x50ED2AAD. Now add that as your ItemID

7. Add The NameID we made in the previous step.

8. Add The Inventory Sort. this is the category your item will go in.
10 = Food
20 = Item
30 = Creature
40 = Soul
50 = Equipment
60 = Key Item
61 = Key Item with Image

9. Add the ItemType. (I haven't documented them so gl). 

10. Set your Offsets. Set Offset2 to the number of your item xi. So item_309.xi would be 309.

11. Set your CarryCap (Max amount of items you can have in a stack. Usually is 99)

12. Set your SellPrice & PurchasePrice. 

13. Set the offset of the ItemPosX & Y. It should be how far across is the item and how far down. Find this based on what we did on the last step.

14. Set your DescriptionID to the textID you made for your item in the previous step.

15. Open ItemIndex and add 1 to the entry group value.

16. Duplicate a entry. Set the ItemID to the ItemID u created. Set the ItemSort to your ItemSort.



there are also Item Equipment and Item Consume Conditions

but i have no clue how those work lmao

ik how u call them but idk how to make them. so i left that out

**BASE ITEM IMAGE:**

![Base item image](https://i.imgur.com/MnvvCkL.png)
