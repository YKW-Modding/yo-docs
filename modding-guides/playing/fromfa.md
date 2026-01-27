---
title: Playing fa Mods (Legacy)
layout: default
grand_parent: Modding Guides
parent: Playing Mods
---

# Playing fa Mods (Legacy)

### Prerequisites
If the mod contains just these (and nothing else):
* `.fa` files
* `mov` folder
* `snd` folder
> Note: It may contain one or more of these - the important thing is that it doesn't contain any *other* files/folders not listed above.

Then this guide is for you! Take all of the files and place them inside a folder called `romfs`.
> Note: It must be called "`romfs`" exactly. No capital letters, spaces or other characters.

Since Yo-kai Watch 1 for Nintendo Switch also uses `.fa` files, a guide for it will be listed here.

## Citra/Azahar/AzaharPlus
This is quite simple!

1. You should have your mod files in your `romfs` folder. Copy the folder.
2. Open your emulator and right click the game you are using. Select "Open Mods Directory".
3. Paste your `romfs` folder here.
4. Launch your mod!

## Ryujinx
1. Set up Ryujinx and Yo-kai Watch 1 for Nintendo Switch.
2. Download the file linked in the mod page for the mod you want. It should open to a folder titled with the name of the mod, or simply a `romfs` folder.
3. Right click on Yo-kai 1 for Nintendo Switch, and click `Open Atmosphere Mods Directory.`
4. If your mod zip has the name of a mod as the folder rather than opening to a `romfs` folder, drag the named folder from the zip into the current directory. (Drag them into the empty space, not over a folder.) If it opens to a `romfs` folder, create a folder in this directory with the name of the mod, and drag the `romfs` folder from the zip into the folder you just created.

## 3DS

This one is a lot more annoying.

1. Make sure your 3DS console is modded. To do so, go to [this website](https://3ds.hacks.guide).
2. Insert your 3DS SD Card into your PC.
3. Go to the `luma` folder and the `titles` folder within it (If any of these folders do not exist, create them).
4. Go to [3dsdb](http://3dsdb.com) and search the title of your game to find its Title ID for example EUR Psychic Specters the TitleID is `00040000001B2900`.
> Remember TitleIDs are *reigon dependant* e.g. the European vs American games will have different Title IDs.
6. In `luma/titles` (where you should be) make a folder and name it the Title ID i.e. a folder called `00040000001B2900`.
7. Copy your `romfs` folder and paste it in the folder you just made. You can now reinsert your SD card into the console.
8. Hold SELECT while booting your console.
9. Use the DPad to navigate down to `( ) Enable Game Patching` and press A. It should now be `(x) Enable Game Patching`.
10. Press START to save and reboot.
11. Launch your mod!
