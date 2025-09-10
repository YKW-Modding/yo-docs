---
title: Editing Textures in YO-KAI WATCH 4
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# Editing Textures in YO-KAI WATCH 4
## Requirements
- [Kuriimu2](https://github.com/FanTranslatorsInternational/Kuriimu2)
- [Viola](https://github.com/SuperTavor/Viola/releases)
- Photo Editing Program

1.   Download Viola, more specifically, Viola.WinForms-Setup.exe, and run it. For ease of use, you can have it open Viola after installing. If you'd like to open Viola from a custom location, the exe will have installed at `C:\Program Files (x86)\Viola.` I personally moved this into my modding hard drive.
2.   Then you'll want to press the Dump button. You'll then be asked for a directory to dump. Navigate to the root of your RomFS dump in Ryujinx if you don't already have everything dumped. If you haven't yet done that, you can so by right-clicking _Yo-kai Watch 4++_ in Ryujinx, hovering over Extract Data, and selecting RomFS.
3.   After this, you'll be asked for an output folder. You can use whatever you want. Every CPK in the game will be unpacked in the folder you choose, so make sure you have plenty of space and time, as it could take a while.

   All textures in the game are controlled by .g4tx files. These files are actually just containers for .nxtch files, but both can be opened, extracted, and imported in Kuriimu2 by dragging the .g4tx file over it.
   Most of the textures you'll see in the game will be in the menu folder, located at `data/nx/menu.` If you want something else, such as textures used for character models, that's in `data/nx/chr/[ModelID].`
   Once you've extracted textures and edited them, simply import them back in and save both the .nxtch and .g4tx file.

   Once you've opened and edited files to your heart's content, (Make sure to save!) there's only one thing left to do: Prepare a romfs directory and pack it.
1.   First, put your modded files in a mod directory with the same filepath as their location in the CPK file. For example, if you edited "title00_01.g4tx," you would set up your directory as `[ModTitle]/romfs/data/nx/menu/50_title/50_title00/title00_01.g4tx` and place the modded file there.
2.   You'll now want to pack your mod. Select the Pack button, and you'll be asked about using an external, vanilla cpk_list. Select Yes, and a window will open that lets you direct Viola to it. In your RomFS dump of the game, the cpk_list.cfg.bin file is in the `data` folder. I personally copied this to the root of my Viola folder to save time searching for it, as well as renaming it to YW4cpk_list.cfg.bin so that I can keep other games' cpk_lists in there.
3.   After that's done, you'll be asked for the target platform, which will be Switch. You'll then be asked for the input path. Select the folder with all your new mod files that you made earlier. For example, if this was the English Mod, I'd select `D:\Mods\Viola 1.3\YW4 EN Translation\romfs` as my path.
4.   Next, you'll have to select your output path. I like to set this directly to where Ryujinx, for example, loads mods so I don't have to worry about copying them over from my hard drive or anything.
