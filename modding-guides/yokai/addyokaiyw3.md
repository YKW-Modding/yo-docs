---
title: Adding Yo-kai (YW3) (CfgBinEditor method)
layout: default
grand_parent: Modding Guides
parent: Yo-kai and battles
---


# Adding Yo-kai (YW3) (CfgBinEditor method)
**original guide by wadi on discord**


## Step 1 - Charabase editing ##
I take it you've already imported your model or you just know what your model id is.

Open ConfigBinEditor and import "chara_base_0.03.25.cfg.bin"
Select the YW3 Tags

Go into "CHARA-BASE-YOKAI-INFO-BEGIN", and duplicate an entry.
Go to this website:
https://emn178.github.io/online-tools/crc32.html and generate a BaseID (Type anything, like MyCustomYokai,JibanyanID or whatever you want.)
Paste that ID you just generated into the BaseID line.

For the FileNamePrefix,
put 5 if your model's first character is x
put 6 if it's y
For the FileNameNumber, if your Yo-Kai's model is y152100, type in 152.
For the FileNameVariant, if your Yo-Kai's model is y152100, you'll type in 10.

For the NameID and DescriptionID:
Open "chara_text_yourlg.cfg.bin"
and follow these instructions : 

For MedalPosX and MedalPosY, use the guide made by MathKK (at the end of the page)

Ranks:

00 - E
01 - D
02 - C
03 - B
04 - A
05 - S

Tribes:

0 - Boss
1 - Brave 
2 - Mysterious
3 - Tough
4 - Charming
5 - Heartful
6 - Shady
7 - Eerie
8 - Slippery 
9 - Wicked
10 - Enma
11 - Wandroid
12 - Boss

Attributes:

0 - Untype
1 - Fire
2 - Water
3 - Lightning
4 - Earth
5 - Wind
6 - Ice
7 - Drain
8  - Strong Attack
9 - Restoration

IsRare, IsLegendary, IsClassic, IsTreaser,... :

0 - No
1 - Yes

Blasters T roles too:
1 - Fighter
2 - Tank
3 - Healer
4 - Ranger

## Step 2 - CharaParam  editing
Part 4 (CharaParam editing)

Generate a ParamID using the same website than charabase
On the BaseID line, use the BaseID value you used earlier.
Set up MinHP, MinStrength, MinDefense, MinSpirit, and MinSpeed.

Do the same with MaxHP, MaxStrength, MaxDefense, MaxSpirit, and MaxSpeed.

Set the MedalliumOffset to 699 (it will break otherwise!)

For Moves, Soulimates, Skills, you can use these from an other yokais or create new

## Step 3 - Hackslash and Battle chara param editing ##
Battle_chara_param is the AI of the yokais (i guess)
Just duplicate one from a existing yokai and paste your Yokai's paramID

Hackslash is the Blasters T Chara_param
You can set your Soulimate, A attack, B attack and Y attack (so moves)