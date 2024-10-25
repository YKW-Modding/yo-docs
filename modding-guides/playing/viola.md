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

1. Set up Ryujinx and either Yo-kai Watch 4++ or Y School Heroes.
1. Download the file linked in the mod page for the mod you want. It should open to a folder titled with the name of the mod, or simply a `romfs` folder.
2. Right click on your game of choice, and click 'Open Atmosphere Mods Directory.'
3. If your mod zip has the name of a mod as the folder rather than opening to a `romfs` folder, drag the named folder from the zip into the current directory. (Drag them into the empty space, not over a folder.) If it opens to a `romfs` folder, create a folder in this directory with the name of the mod, and drag the `romfs` folder from the zip into the folder you just created.

## Merging Mods

This is a method that involves taking multiple mods that you want to use and merging them into one mod.

1. Download Viola, extract the contents of the zip into a folder, and open a CMD window in that folder. If you don't know how, the easiest way is to go into the folder where you have Viola extracted, click the search bar, and type "cmd."
2. Make sure you have all the mods you want to merge downloaded and extracted into a folder. Type into Viola, `viola merge "<Folder Directory>" "<Folder Directory"`, and you will get a prompt for where you want the merged mod to be placed. As an example, here's what I do for merging the Yo-kai Watch 4++ English Fan Translation and the Unrestricted Crank mods:
   `viola merge "C:\Users\Light\AppData\Roaming\Ryujinx\sdcard\atmosphere\contents\010086C00AF7C000\Yo-kai Watch 4++ English Translation\romfs" "C:\Users\Light\AppData\Roaming\Ryujinx\sdcard\atmosphere\contents\010086C00AF7C000\Unrestricted Crank\romfs"`
3. Put it into whatever folder you'd like when prompted to do so; I prefer to put them in a mod called Merged.
4. When it's finished, you'll get a notice about what command to use if you want to pack your mod, which is what you want to do. The only problem with the command given is that the folder directory it gives you is not in quotes, which is necessary. So copy it before the folder directory, manually enter quotation marks, copy the directory, and end it with quotation marks again. For me, this was the command to do:
   `Viola.exe pack "C:\Users\Light\AppData\Roaming\Ryujinx\sdcard\atmosphere\contents\010086C00AF7C000\Merged\romfs"`
5. Once your mod has finished packing, there will be a folder called `romfs_output_mod` around where you specified it to pack. Copy the `data` folder inside this folder, and paste it in the `romfs` folder for the mod. If there is no romfs folder for the mod, make it.
6. Whichever mod you used to put your merged stuff in is the one you should then enable when loading mods. The only other mods you should have enabled aside from your merged one are code mods, such as 60FPS and 120FPS mods, or the Fullwidth Patch mod.
