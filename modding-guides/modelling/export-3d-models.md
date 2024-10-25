---
title: How to Export 3D Models from the Games to Standard Formats?
layout: default
parent: Models and Animations
grand_parent: Modding Guides
---

# How to Export 3D Models from the Games to Standard Formats?
**Original author on Discord: @whisperito**

Hello, today I'm explaining you how you can export 3D models from the games (Currently only the 3DS and Switch games) 

# 3DS Games:

## Software:

__**For This You'll Need:**__
- [Kuriimu2](https://github.com/FanTranslatorsInternational/Kuriimu2)
- [Metanoia (fork by Tiniifan)](https://github.com/Tiniifan/Metanoia/)
- An extracted RomFS, if not, check this [guide](https://discord.com/channels/1053460697754898432/1146817437526929528) by Woganog


__**Step 1: Extracting the Raw Model from the Game**__

1. Open your main FA in Kuriimu2 (it looks like this `yw2_a.fa`, depending on the game, it may be a 1 or missing, it's normal.)
2. Go to the `character` folder (scroll the folder)
3. Find your model (eventually using the [File Naming](https://gamebanana.com/tools/11464) project, but it only contains scoutable Yo-Kai and Yo-Criminals, if you need something else, like a human or a boss, ask in <#1053460755833426061> or eventually [this method](https://discord.com/channels/1053460697754898432/1053460755833426061/1157804234281070642) but you may not understand it in the first place)
4. Once you found your model ID, click on the folder, you'll see that it contains multiple .xc files, take only this one `[modelID_p00.xc]` and extract it.

__**Step 2: Exporting the Model**__

1. Open Metanoia.
2. Open your previously extracted file with Metanoia and then click "Export as model"
3. Then, choose the file type and name and confirm.
4. You'll be prompted "Export materials and textures?", click Yes.
5. You're done! Enjoy your model!

# Switch Games:

## Software:

__**For This You'll Need:**__
- [Noesis](https://richwhitehouse.com/index.php?content=inc_projects.php&showproject=91)
- [CriFsLibGUI](https://github.com/Sewer56/CriFsV2Lib/releases)
- [This Noesis Addon](https://cdn.discordapp.com/attachments/1053460755833426061/1196766889028882442/yokai_watch_4_switch.py?ex=6602a705&is=65f03205&hm=0793df581a9ed770d50f77b923d26f088c91e160b3d619c55067c4d8b5574d3c&)
- An extracted RomFS, if not, just right click your game in Ryunjix / yuzu and click on "Extract Data > RomFS" (Ryunjix) or "Dump RomFS" (yuzu)

__**Step 0: Setting up a Few Things**__

1. Once you extracted Noesis from its folder, open its folder and go to plugins, then Python and put the addon in it.
2. Go back to the extracted RomFS, and go to `../data/common/chara`, you'll see a `chara.cpk` file, open CriFsLib and drag the `chara.cpk` onto it. Then, right click and click "Extract All", a `chara` folder should be created.

__**Step 1: Exporting the Model**__

1. Search the Yo-kai whose model you want to export using this [list](https://docs.google.com/spreadsheets/d/1JABcMLPR1lp2cjappmUGu_1kkLk2wTXtklrH5WlZcBM/edit#gid=0) (big thanks to Light)
2. Once you have its model ID, open the `chara` folder that has been created earlier, then, browse to `/data/nx/chr/`, and search the folder of your Yo-kai (by Model ID).
3. Then, copy all of the `.g4tx` files, then, go back to the `chara` folder, and go to `data/common/chr`, then, as earlier, search the folder of your Yo-kai (by Model ID) and paste the previously copied file in its folder
4. Open Noesis, then, File > Open, and go to your extracted RomFS, then, to the folder of your Yo-kai (in the `common` folder), and open the `[modelID].g4pkm`.
5. Then, do File > Export from Preview. I recommend to check Flip UVs (otherwise you'll have to do it afterwards), then, choose the output file type and output texture file type (default is .png)
