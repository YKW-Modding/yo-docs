---
title: Adding New Yo-kai to Yo-kai Watch 4
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---


# Adding New Yo-kai to Yo-kai Watch 4
**Original guide by 8227light on Discord**

Before I begin this guide, note that **at this time, you must have a model already in the game's format.** There is no model importer, so you can only have existing Yo-kai or a texture edit of an existing Yo-kai.

Furthermore, the **actual usability is very limited at this time.** It is currently unknown how animations fully work, and it has been identified that the model itself has data that allows them to attack, however the method for which to generate data for existing Yo-kai, much less new Yo-kai, is completely unknown. If you try to add a Yo-kai like Blizzie, what you'll get is a useless Yo-kai that just looks cool and can't do anything.

# Required Files

You will only need 3 files right now if you want to do this, and only 2 if you don't need to/don't want to add text for Yo-kai joining you:
**chara_base_0.00.00.cfg.bin**
**chara_param_0.13.62.cfg.bin**
**addmenber_text.cfg.bin**

# chara_base Edits
Open the file in CfgBinEditor after downloading the latest MyTags.
Skip the CHARA_BASE_BATTLE_LIST struct and move on down to the CHARA_BASE_INFO_LIST struct.
First, check that your Yo-kai doesn't already have a chara_base entry. This can be easily done by searching the filename string, such as y02870000. If there is an entry already, perfect! If not, simply duplicate an existing entry so you only have to tweak some values.
## If you had to make a new entry
In this case, you will need to generate a BaseID. This can be done here: https://emn178.github.io/online-tools/crc32.html You can put whatever you want in the input, just copy the hex output and put it in the BaseID variable. Make sure to also edit the EntryCount to account for your new entry! This can be edited by clicking on CHARA_BASE_INFO_LIST, the one with an arrow by it. 

# chara_base Edits Continued
Next, make sure that the ModelID variable is set to the filename string of your model, as said earlier.
Lastly, you can also set the name that your Yo-kai uses ingame with the NameNounID variable. If you have no idea what NounID you need to change it to, open **chara_text.cfg.bin** in CfgBinEditor, find the name you want to use, copy the NounID from there, and paste it into the NameNounID variable.

Next comes setting up a CHARA_BASE_INFO_REF_BATTLE entry. This lets the file know what to reference in another struct, which contains data for Role, Rank, and Tribe. The BASEBATTLERef variable refers to the entry number of CHARA_BASE_BATTLE_LIST that your Yo-kai will reference. A BASEBATTLERef value of 84, for example, will reference entry 84 of CHARA_BASE_BATTLE_LIST. The TypeID variable controls if the entry in CHARA_BASE_INFO_LIST will reference CHARA_BASE_BATTLE_LIST or not. You can set it to 0 to make it not reference an entry, but since we do, set it to 1.
If you edited a preexisting CHARA_BASE_INFO entry, you don't have to worry about making a CHARA_BASE_INFO_REF_BATTLE entry, but if you made a new CHARA_BASE_INFO entry, you'll have to do the same for a CHARA_BASE_INFO_REF_BATTLE entry. If you made a new CHARA_BASE_INFO_REF_BATTLE entry, but already edited the EntryCount for the CHARA_BASE_INFO entry, you do not edit the EntryCount.

Now we can move to the final step for this file, the CHARA_BASE_BATTLE_LIST entry.

Since this is a new Yo-kai, we'll want to add a new CHARA_BASE_BATTLE entry, so do so. I once again recommend duplicating an entry from a preexisting Yo-kai so you only have to tweak some things. Similarly, don't forget to edit the EntryCount for this struct.
### If you didn't do so earlier, set the BASEBATTLERef value to that of the entry number you just made. For example, if I made a new entry called CHARA_BASE_BATTLE_355, the BASEBATTLERef value should be 355.

From here, what you need to edit is simple. (Or at least, what is documented is simple.) The variable names are very self-explanatory, so all you should need is a guide for what correlates to what.

**Role:** 1 - Attacker, 2 - Shooter, 3 - Tank, 4 - Healer, 5 - Watcher
**Tribe:** 1- Goriki, 2 - Onnen, 3 - Mononoke, 4 - Tsukumono, 5 - Uwanosora, 6 - Omamori, 7 - Mikakunin, 8 - Mikado, 9 - Izana, 10 - Oni, 11 - Wicked, 12 - Shinma
**Rank:** E Rank - 1, D Rank - 2, C Rank - 3, B Rank - 4, A Rank - 5, S Rank - 6

# chara_param Edits

There's a high likelihood you'll have to make a new entry for this file. If you'd like to check regardless, see if the BaseID from before is used in this file. If not, or if you had to make a new chara_base entry in the first place, you'll have to make a new entry in CHARA_PARAM_INFO_LIST. As always, edit the EntryCount if you add a new entry!
For the ParamID variable, use the same website linked for making a new BaseID previously.
For the BaseID variable, use the BaseID you have on your new Yo-kai in the chara_base file.
The Min/Max stat variable names are pretty self-explanatory, with just one caveat: These Min/Max stats only apply to Levels 1-99.
ElementResist and ElementWeak refer to which Elements the Yo-kai are strong or weak to. You can find a key for them below.
Similarly, Speed refers to how fast they are, with a key also found below.
CharaType obviously refers to what kind of character this entry is. If it's not already there, put 'yokai'.
TransformsInto is only necessary if your Yo-kai is a Shadowside Yo-kai. If this is the case, put the ParamID of what they transform or revert into. Otherwise, leave it as 0.
Min/MaxYP follows the same rules as the other Min/Max stat variables.
The 120 Stat variables are what that stat will be at Level 120. 

Finally, let's move onto the last needed struct, YOKAI_PARAM_INFO_LIST. This contains moveset data.
Like before, you'll likely have to make a new entry, but you can double-check the ParamID if you so choose. Otherwise, I recommend duplicating a preexisting entry. And you know the drill by now... Edit the EntryCount if you added a new entry.
Obviously, put your new Yo-kai's ParamID in the ParamID variable.
SkillID, YMoveID, SoultimateID, and the XMoveID variables are pretty self-explanatory. List the ParamIDs for each respective area.
The XMoveUnlock variables refer to the level at which the Yo-kai will unlock that XMove. If you want it to be innately learned for example, set it to 0.

**Elements:** 1 - Fire, 2 - Water, 3 - Lightning, 4 - Earth, 5 - Ice, 6 - Wind, 7 - Light, 8 - Dark
**Speed:** 0 - Normal, 2 - Slow, 3 - Fast

You can find ParamIDs for movesets and more here: https://docs.google.com/spreadsheets/d/1JABcMLPR1lp2cjappmUGu_1kkLk2wTXtklrH5WlZcBM/edit?usp=sharing

# addmenber Edits

This file pertains to dialogue used after getting a Yo-kai in the Crank-a-Kai or through Soulmates.
Thankfully, this part is ridiculously simple.
Add a new entry, and make sure to edit the EntryCount.
For ParamID, list the ParamID of the Yo-kai you want the dialogue to appear for.
TypeID refers to whether it's Crank-a-Kai or Soulmates text. Use 0 for Crank-a-Kai, and 1 for Soulmates.
In Text, simply write whatever your heart desires for this character to say.

As of 7/4/24, this is the most we can do for now.
Should any further developments be done, or everything is fully figured out, I will edit the above guide and remove this part.
