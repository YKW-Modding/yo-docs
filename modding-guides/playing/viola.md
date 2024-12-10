---
title: Playing Viola Mods (Yo-kai Watch 4/Gakuen)
layout: default
grand_parent: Modding Guides
parent: Playing Mods
---

# Playing Viola Mods (Yo-kai Watch 4/Gakuen)

### Prerequisites

[Viola](https://github.com/SuperTavor/Viola/releases)

If a mod is formatted using loose files (There are individual .cfg.bin files or otherwise with no .cpk files), then take all of the files and put them inside a folder called `romfs`. If they are in a .cpk format, you will have to convert it to a loose format so more mods can be used with each other without issue, while also being smaller. If you want to use multiple mods, you'll have to merge them.
This only applies to Yo-kai Watch 4++ and Yo-kai Watch Jam: Y School Heroes: Bustlin' School Life. Yo-kai Watch 1 for Nintendo Switch does not use CPK files, and instead uses an FA like the 3DS games.

## Ryujinx

1. Set up Ryujinx and either _Yo-kai Watch 4++_ or _Y School Heroes_.
2. Download the file linked in the mod page for the mod you want. It should open to a folder titled with the name of the mod, or simply a `romfs` folder.
3. Right click on your game of choice, and click 'Open Atmosphere Mods Directory.'
4. If your mod zip has the name of a mod as the folder rather than opening to a `romfs` folder, drag the named folder from the zip into the current directory. (Drag them into the empty space, not over a folder.) If it opens to a `romfs` folder, create a folder in this directory with the name of the mod, and drag the `romfs` folder from the zip into the folder you just created.

## Merging Mods

This is a method that involves taking multiple mods that you want to use and merging them into one mod.

1.   This is a method that involves taking multiple mods that you want to use and merging them into one mod. First, download Viola, more specifically, Viola.WinForms-Setup.exe, and run it. For ease of use, you can have it open Viola after installing. If you'd like to open Viola from a custom location, the exe will have installed at `C:\Program Files (x86)\Viola.` I personally moved this into my modding hard drive.
2.   Next, make sure you have all the mods you want to merge downloaded and extracted into a folder. I prefer to put them in folders where I have my Viola install, so I can more easily modify things without permission issues and easily pack mods. Press the Merge button, and you'll be prompted for the number of mods you want to merge.
3.   After inputting the number, you'll then be prompted for the directories of said mods. For example, I'll be merging the English Mod and the Unrestricted Crank mod. First, I selected my folder path for the English Mod, (`F:\Mods\Viola\YW4 EN Translation\romfs`) then my folder path for the Unrestricted Crank mod. (`F:\Mods\Viola\Unrestricted Crank\romfs`)
4.   You'll then be asked for the output path. I like to set this directly to where Ryujinx, for example, loads mods so I don't have to worry about copying them over from my hard drive or anything. Viola will automatically create the romfs folder and everything for you.
5.   Next, it'll ask for an external cpk_list file. This can be found by right-clicking _Yo-kai Watch 4++_ in Ryujinx, hovering over Extract Data, and selecting RomFS. Or, if you already have a RomFS dump of the game, the cpk_list.cfg.bin file is in the `data` folder. I personally copied this to the root of my Viola folder to save time searching for it, as well as renaming it to YW4cpk_list.cfg.bin so that I can keep other games' cpk_lists in there.
6.   After that's done, you'll be asked for the target platform, which will be Switch. Then, your mods will start being merged. This could take a while, so just be patient.
7.   When it's finished, your mod should be in your selected output folder all ready to go. If you set the outpath to an external directory, you'll just need to set it up like a proper mod in Ryujinx and such. If you don't know what that looks like, use other mods for an example!
