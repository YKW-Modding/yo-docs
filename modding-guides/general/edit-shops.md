---
title: Shops (YW2)
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# Editing Shops (YW2)
> This tutorial is made for YW2, params *will* change between games so this won't accurately apply to other games.

First, we need to find the shop you want to edit, navigate over to `data/res/shop` and open the correct `cfg.bin`. 
There are two kinds of shops: special shops and normal shops. This guide will cover both. A list of the special shops in Yo-kai Watch 2 can be found below:
* `creature_reward_config.cfg.bin` - This controls Jungle Hunter milestones and will also be covered here.
* `def_shoplist.cfg.bin` - Holds the list of shops - to add a new one increase the `ChildCount` in `SHOP_LIST`, duplicate a new `SHOP_LIST_INFO` and add your new `ShopID` to it (the CRC-32 of the Shop Name).
  * `combine_config.cfg.bin` - This controls fusions and will not be covered here as a seperate tutorial covers this.
* `another_shop.cfg.bin` - This depends on the game but for Yo-kai Watch 2, it's the bicycle shop; the bicycle shop will not be covered here as it's too complicated.
Normal Shops will be in the format `shop_<shopName>_0.01n.cfg.bin` - DO NOT edit the ones without `_0.01n` as they are use a different format, are older, and will be ignored by the game.
A normal shop's `ShopName` is in the following format:
* `shp<type><index>`
  * `<index>` is a 3-digit number used to distinguish between shops of the same type.
  * `<type>` is a capitalised 1/2 letter sequence used to show the type of shop:
    * `N` is used for normal shops - this is the most common shop type.
    * `M` is used for Vending Machines - there are only a handful of these.
    * `KP` is used exclusively for the BP (Battle Points) shop.
    * `TVM` is used exclusively for the Day Pass "shop".
    * `B`
    * `BB`
    * `BC`
    * `PAY`
    * `T`

## Normal Shops
### Adding a new Item
First, in the `SHOP_CONFIG_INFO` tree, increase `ChildCount` by 1. Then:
* Duplicate the last `SHOP_CONFIG_INFO`.
* Change the `ShopSlotID` to a random CRC-32, you can do this by going to a website like [this](https://emn178.github.io/online-tools/crc/) and typing anything into the textbox there.
* Change the `ItemID` to whatever Item you want it to give, a list of `ItemID`s can be found [here](https://ykw-modding.github.io/yo-docs/modding-resources/item-ids/YW2ItemIDs.html).
  * If you want this item to have a limited number of purchases per in-game day, set `HasLimitedStock` to 1 and edit `Stock`, otherwise set `HasLimitedStock` to 0.
* Increase `shpIndex` by 1 and set the last param (`HasCond`) to 1.
* Next, increase the `ChildCount` of `SHOP_VALID_CONDITION`; if it dosen't have this then you are editing the wrong shop.
* Duplicate the last `SHOP_VALID_CONDITION`.
* Now, in this new `SHOP_VALID_CONDITION`, change the `Price` to the price of the item.
  * This is usually in the main currency (internally always yen, so $/£/€1.10 = 110yen, 250w = 100y), although sometimes it's non-primary currencies such as JP, BP/KP etc.
* Finally, change the `Cond` to the Cond you want for the shop item - if you always want it to be available set the `Cond` to 0 (make sure it's an Integer!) and save your changes.

### Adding a new Shop (W.I.P)
> This section has not been tested yet, and may not work.
> Additionally, this section is for Normal Shops; you cannot create new Special Shops, only edit/delete ones that are already programmed into the game.

First, create a shop name i.e. `shpN005` for the 5th normal shop; *this name must not already be taken*. Then:
* Go to `def_shoplist.cfg.bin`, click on `SHOP_LIST` and increase `ChildCount` by 1.
* Then duplicate the last `SHOP_LIST_INFO` and select the newly created `SHOP_LIST_INFO`.
* Type your `ShopName` into a website like [this](https://emn178.github.io/online-tools/crc/), and set the `ShopID` to the output the website gives you.
* Then find a `_0.01n` `cfg.bin` that's similar to your shop.
  * Duplicate it and rename it to your shop name, following the pattern of all the other Normal Shops.
* In the `SHOP_CONFIG_INFO` of your new shop change the old `ShopID` to the `ShopID` of the new shop you placed in `def_shoplist` earlier.
* Change all the `ShopSlotID`s to unique random CRC-32s, you can get them by going to a website like [this](https://emn178.github.io/online-tools/crc/) and typing anything into the textbox there.
* Finally, save your changes.

## Special Shops

### Adding a new Jungle Hunter Milestone
In `data/res/shop`, open `creature_reward_config.cfg.bin`. Then:
* If you want a bug milestone focus on `CREATURE_BONUS_ITEM_LIST_0`, if you want a fish milestone focus on `CREATURE_BONUS_ITEM_LIST_1`.
* Once you've picked your list, increase it's `ChildCount` by 1.
* Then duplicate it's last entry.
  * Set `Milestone` to the amount of unique types of Bugs/Fish that need to be caught, or `255` if you want all of them to be required.
  * Set `ItemID` to the ID of the Item you want it to reward, a list of `ItemID`s can be found [here](https://ykw-modding.github.io/yo-docs/modding-resources/item-ids/YW2ItemIDs.html).
    * Tip: If you want to make it give you 2 Items, create 2 Milestones with the same `Milestone` requirement.
  * Set `Quantity` to the amount of the Item it should give.
  * Set `UnkID` to a random CRC-32, you can do this by going to a website like [this](https://emn178.github.io/online-tools/crc/) and typing anything into the textbox there.
  * Finally, save your changes. 
