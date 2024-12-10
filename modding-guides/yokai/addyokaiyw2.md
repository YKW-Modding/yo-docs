---
title: Adding Yo-kai (YKW2) (CfgBinEditor Method)
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---


# Adding Yo-kai (YW2) (CfgBinEditor Method)
**Original guide by reese on discord**


This guide will be updated as time goes on!


# This guide contains all necessary information on adding, editing, and configuring new yo-kai.
# Step 0 (Preparation)
Start by downloading the .zip file linked in the google drive containing all necessary applications for modifying the game and adding new yo-kai.
https://drive.google.com/drive/folders/1JWQTRxVyu0CyMaEfbCWchpRYzFWDv0Tf?usp=sharing
You will need a romFS dump of your Yo-Kai Watch 2 game (right click on the title in Citra and click "dump romFS")
With that dump, take the .fa file with **no** language extension (no en, es, fr, jp, etc.)

Take that romFS and duplicate it into a seperate folder. Keep the first romFS for data recovery & ease of access (like if the fa gets corrupted or you make a big mistake.)
Head to data > res > character and extract these two files:
> chara-base-0.04c.cfg.bin
> chara-param-0.03a.cfg.bin
Install Tiniifan's Level-5 Blender Addon (to export the model)

# Step 1 (Getting the Custom Yo-Kai Ready)

Open **Kuriimu** and get the data for your preferred Yo-kai and extract the folder.
With this, you will open the **Metanoia** application, look for the **import** option, and import the folder. The "Folder" we're looking for is the .xc that you extracted earlier. Once that is imported, export it as a .obj and edit it in Blender. The next step is to export the model as a .xc. 
# WHEN ON THE EXPORT MENU, GO TO TEMPLATE AND SELECT MESH (PRM)! **THE MODEL WILL NOT WORK OTHERWISE!**
# **FROM THERE, EXPORT USING THE YKW2 TEMPLATE!**

# Step 2 (Importing the Yo-Kai)

Once you've exported the file, open the **Kuriimu** application and go to the folder the most similar in animation to your yokai (ex: if you're making a jibanyan variant, export jibanyan at y152000)
Change all of the IDS of your exported model to another one that isn't taken (eg: Jibanyan's ID is y152000, so change all the y152000s to y152100s)
Now import the new yo-kai using **XFSAManager**.
Open the yw2_a.fa file, and go to 
 data / character 
Right click on a blank space, click on folder, then click on import.
Wait for it to import, then save the .fa file.
If your custom Yo-Kai is a texture swap:
Open it in Kuriimu, then open the folder of your custom yo-kai.
Edit the image and replace it, then **save as** the folder in a seperate, accessible location.
If your custom Yo-Kai is a fully new model:
Replace the p00 file by your exported model file, and **save as** the folder in a seperate, accessible location.
For replacement,
Go to the folder of your custom yo-kai and replace it with your seperate folder and save your changes.

# Step 3 (CharaBase Editing, Adding in your New Yo-Kai)

Open CfgBinEditor and import  "chara-base-0.04c.cfg.bin"
For the template, select YKW2.
From here, go into "CHARA-BASE-YOKAI-INFO-BEGIN", and duplicate an entry.
Head to this website: https://emn178.github.io/online-tools/crc32.html and generate a BaseID (Type anything, like MyCustomYokai, or whatever you want.)
Paste that ID you just generated into the BaseID line.

For the FileNamePrefix,
put **5** if your model's first character is **x**
put **6** if it's **y**

For the FileNameNumber, if your Yo-Kai's model is y152100, type in 152.
For the FileNameVariant, if your Yo-Kai's model is y152100, you'll type in 10.
For the NameID and DescriptionID:
Open **Albatross** and open your extracted romFS.
Search the name of the yo-kai you want to use, and copy their BaseID.
Search it on CharaParam and copy the BaseID.
Once you've found it, copy both the NameID and DescriptionID.
**Full NameID customization is planned soon.**

For MedalPosX and MedalPosY, use the guide made by math_kk (the image is located in your folder!)

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

IsRare, IsLegendary, and IsClassic:

0 - No
1 - Yes

CharacterBase is now fully configured!

# Step 4 (CharaParam Editing)

Generate a ParamID using the previous website.
On the BaseID line, use the BaseID value you used earlier.
Set up MinHP, MinStrength, MinDefense, MinSpirit, and MinSpeed.

Do the same with MaxHP, MaxStrength, MaxDefense, MaxSpirit, and MaxSpeed.

Set the MedalliumOffset to 449 (it will break otherwise!)

MoveIDs are configured with Albatross.
