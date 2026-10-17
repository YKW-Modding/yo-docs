---
title: Adding Yo-kai (YW4)
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---

# Adding Yo-kai (YW4)
**Original guide by 8227light on Discord**

Before I begin this guide, note that **this guide assumes you have a model already in the game's format.** There is a model tool, but I have yet to utilize it for this purpose.

# Required Files

The files you need may vary depending on what you are trying to do, ie whether you're adding an existing Yo-kai or doing a recolor. For this guide, I'll act as if you're doing a recolor of Present Noko.

**[ModelFilename] folder files:** This is for everything related to the model, found in data/common/chr and data/nx/chr.

**chara_base_0.00.00.cfg.bin:** Assigns some core info, such as Tribe and Rank.

**chara_model.cfg.bin**: Assigns an objbin to your Yo-kai.

**chara_motion.cfg.bin**: Assigns g4pk files to your Yo-kai.

**chara_param_0.13.62.cfg.bin:** Assigns some core info, such as stats and moves.

**chara_scale_4.00.23.cfg.bin**: Assigns scaling details to your Yo-kai.

**addmenber_text.cfg.bin:** Assigns dialogue the Yo-kai says when befriending you, for both the Crank-a-Kai and Konkatsu.

**chara_text.cfg.bin:** Assigns a name to your Yo-kai.

**chara_menu_resource.cfg.bin**: Assigns icon resources to your Yo-kai.

# chara_text: NOUN_INFO

Open the file in CfgBinEditor after downloading the latest MyTags.


Duplicate the entry of the Yo-kai you're recoloring, and change the string for their name.

You'll want to make a new NounID here, which you can generate at https://emn178.github.io/online-tools/crc/. This NounID is just a CRC hash of 'name_[ModelName]'. So, for this guide, I'll be hashing 'name_y02210010'.

# chara_base: CHARA_BASE_INFO_LIST

Open the file in CfgBinEditor after downloading the latest MyTags.

Skip the CHARA_BASE_BATTLE_LIST struct and move on down to the CHARA_BASE_INFO_LIST struct.

Duplicate the entry of the Yo-kai you want to base your new one off of, and make a new BaseID. BaseIDs are just a CRC hash of the model name, and for this guide, I'll make my Noko recolor's model name y02210010.

The Filename should be pretty self-explanatory. It's the name used in the actual files. For this guide, y02210010.

The NameID here will be the same as the NounID that you made previously for chara_text.

You'll want to make a new CharaModelID. CharaModelIDs are just a CRC hash of 'mdl_[ModelName]'. So, for this guide, I'll be hashing 'mdl_y02210010'.

In the CharaMotionID section, set it to your new BaseID. They're the same.

In the MenuResourceID section, set it to your new BaseID. They're the same.

Next comes setting up a CHARA_BASE_INFO_REF_BATTLE entry. This lets the file know what to reference in the other struct in this file, CHARA_BASE_BATTLE_LIST, which contains data for Role, Rank, and Tribe. CharaBaseBattleStartPos refers to the entry number in the aforementioned struct that it will start looking in, and CharaBaseBattleLength is how many entries from that point it will look at, which will always be 1 for this. Duplicate a REF_BATTLE entry and configure it so it references the correct entry.

# chara_base: CHARA_BASE_BATTLE_LIST

For our last steps in this file, duplicate the BATTLE_LIST entry of the Yo-kai you're basing your new one off of. Note the entry number in the list after doing so, as that will be the number you should be putting in for the CharaBaseBattleStartPos in the other struct.

Role, FavoriteFood, DislikedFood, Tribe, and Rank are pretty self-explanatory, as long as you know what number correlates to what. You can find a key below.

**Food**:
  - 0: No Food
  - 1: Rice Balls
  - 2: Bread
  - 3: Candy
  - 4: Milk
  - 5: Juice
  - 6: Burgers
  - 7: Ramen
  - 8: Sushi
  - 9: Veggies
  - 10: Fruit
  - 11: Meat
  - 12: Seafood
  - 13: Curry
  - 14: Sweets
  - 15: Soba
  - 16: Udon
  - 17: Snacks
  - 18: Chocobars
  - 19: Ice Cream
  - 20: Doughnuts
  - 21: Tempura
  - 22: Sukiyaki

**Role:**
  - 0: No Role
  - 1: Attacker
  - 2: Shooter
  - 3: Tank
  - 4: Healer
  - 5: Watcher

**Tribe:**
  - 0: No Tribe
  - 1: Goriki
  - 2: Onnen
  - 3: Mononoke
  - 4: Tsukumono
  - 5: Uwanosora
  - 6: Omamori
  - 7: Mikakunin
  - 8: Mikado
  - 9: Izana
  - 10: Oni
  - 11: Wicked
  - 12: Shinma

**Rank:**
  - 0: No Rank
  - 1: Rank E
  - 2: Rank D
  - 3: Rank C
  - 4: Rank B
  - 5: Rank A
  - 6: Rank S

Once you're done, make sure to update the ChildCounts of each struct by clicking the dropdown.

# chara_param: CHARA_PARAM_INFO_LIST
Duplicate the entry of the Yo-kai you're basing your new one off of, and make a new ParamID. ParamIDs are a CRC hash of 'para_[ModelName]'. So, for this guide, I'll be hashing 'para_y02210010'.

Use the BaseID you made earlier for the BaseID section.

You'll see various stats prefixed with BaseA/B/C. BaseA/B correlate to base stats around Lv1 and Lv99, while BaseC correlates to base stats around Lv120.

ElementResist and ElementWeak refer to which Elements the Yo-kai are strong or weak to. You can find a key for them below.

Speed refers to the tier of speed that Yo-kai will have. You can find a key for it below.

CharaType should be set to 'yokai' already. If it isn't, change it.

TransformsInto should only be necessary if you are making a Shadowside Yo-kai or assigning what a transformation should revert back into. If this is the case, put the ParamID of what they transform or revert into. Otherwise, leave it as 0 for them to undergo Great Change.

**Elements:**
  - 1: Fire
  - 2: Water
  - 3: Lightning
  - 4: Earth
  - 5: Ice
  - 6: Wind
  - 7: Light
  - 8: Dark

**Speed:**
  - 0: Normal
  - 1: Very Slow
  - 2: Slow
  - 3: Fast
  - 4: Very Fast

# chara_model/chara_motion
Duplicate the entry of the Yo-kai you're recoloring.

Change the CharaModel/MotionID to the one you used in chara_base.

Change the Filepath to where your objbin/g4pks are located.

# chara_param: YOKAI_PARAM_INFO_LIST
Duplicate the entry of the Yo-kai you're recoloring.

Put in your newly made ParamID.

The different MoveIDs, SkillID, and SoultimateID use the ParamIDs for said moves. Don't change these unless you want a different moveset, of course, but if you do, look and see what the other IDs for moves are.

The XMoveUnlock variables refer to the level at which the Yo-kai will unlock that XMove. If you want it to be innately learned, for example, set it to 0.

Once you're done, make sure to update the ChildCounts of each struct by clicking the dropdown.

# chara_scale
Duplicate the entry of the Yo-kai you're recoloring.

Change the CharaID to your new BaseID. Since this cfgbin is a new type of cfgbin, names are set per-file. For this file, CharaID is the same as the BaseID.

# chara_menu_resource
Duplicate the entry of the Yo-kai you're recoloring.

Change the MenuResourceID to the one you created.

Change the various filepaths to whichever ones you're using for your new Yo-kai.

# addmenber_text: TEXT_INFO
Duplicate the entry of the Yo-kai you're recoloring.

In the TextID section, put your new ParamID. They're the same.

TextIndex refers to whether it's for the Crank-a-Kai or Konkatsu. 0 is for the Crank-a-Kai, and 1 for Konkatsu.

TextString is obvious. Write what you want them to say.

# Model Files (The Annoying Part)
As a warning, this part will be extremely tedious, as we lack anything to automate it, and as such is manual.

Let's start with data/common/chr. In the g4pkm, you'll see a bunch of strings that explicitly list the model's filename, for example, y02210000. Change these to your new model's filename, but also *change the hashes*. There are hashes for the strings already listed in the g4pkm, plus ones for the texture. These are easy to change, just search the hash, generate a new hash matching the new filename string, and use that new hash instead. If you aren't sure what name hashes are in the texture, open the texture in Kuriimu2 and search the hash of the names of the nxtch files inside of it.

Repeat this tedious process of changing explicit strings and hashes for the g4pkm and g4pk files. Repeat this same process for the texture file inside data/nx/chr. However, there's one thing to watch out for there.

If you edit one of the textures, there's a chance the hash for it may be reordered. Just in case it does, here's how you can check it. Above all the strings for the nxtch filenames, there is all the hashes for the names, as well as an index for the order. In the unmodified y02210000 texture, the index follows 1-2-0, for y02210000_10, y02210000_10line, and y02210000_10oc. This order is important, as the hashes must be ordered correctly. Changing the hashes naturally means they wouldn't be in the same order, so you'll likely need to reorder the index to follow the new hash order.

Make sure to follow the process for the model texture for any other textures you modify, such as the menu textures.
