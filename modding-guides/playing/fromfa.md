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
> Note: It should be called "`romfs`" exactly. No capital letters, spaces or other characters.

Since Yo-kai Watch 1 for Nintendo Switch also uses `.fa` files, a guide for it will be listed here.
> The mobile variant is different, please follow the [Smartphone Guide](./smartphone.html) if you want to play a mobile mod.

## Citra/Azahar/AzaharPlus
This is quite simple!

1. If you have a `romfs` folder, copy it. If not, place them inside one,
2. Open your emulator and right click the game you are using. Select "Open Mods Directory".
3. Paste your `romfs` folder here.
4. Launch your mod!

## Ryujinx
1. Set up Ryujinx and Yo-kai Watch 1 for Nintendo Switch.
2. Download the file linked in the mod page for your corresponding mod. It should open to a folder titled with the name of the mod, or simply a `romfs` folder.
3. Right click on Yo-kai 1 for Nintendo Switch, and click `Open Atmosphere Mods Directory.`
4. If your mod zip has the name of a mod as the folder rather than opening to a `romfs` folder, drag the named folder from the zip into the current directory. (Drag them into the empty space, not over a folder.) If it opens to a `romfs` folder, create a folder in this directory with the name of the mod, and drag the `romfs` folder from the zip into the folder you just created. If neither of these are true, place your mod files inside a `romfs` folder.

## 3DS

This one is a lot more annoying.

1. Make sure your 3DS console is modded. To do so, go to [this website](https://3ds.hacks.guide) if it is not already.
   - If you are unsure, the first step of this process, should tell you if your 3DS is modded.
   - Modding your 3DS is *safe*, any loss is entirely user error, and is extremely rare to encounter unintentionally.
3. Insert your 3DS SD Card into your PC.
4. Go to the `luma` folder and the `titles` folder within it (If any of these folders do not exist, create them).
5. Go to [3dsdb](http://3dsdb.com) and search for your corresponding game, and region to find its `TitleID`. For instance, the European variant of Psychic Specters has a `TitleID` of `00040000001B2900`.
> Remember TitleIDs are *region dependant* e.g. the European vs American games will have different Title IDs. Both Checkpoint and FBI can be used to find the `TitleID` of your title, if you aren't sure of the region, although you can usually tell from the symbol of the currency:
  - $ = USA/NA
  - ¥ = JPN
  - ₩ = KOR
  - £, €, etc = EUR
6. In `luma/titles` (where you should be), create a folder with its name being the `TitleID` of your title. For instance, using our earlier example the folder would be called `00040000001B2900`.
7. Copy your `romfs` folder, and paste it into the folder you have just made. You can now reinsert your SD card into the console.
8. Hold SELECT while booting your console.
9. Use the D-Pad to navigate down to `( ) Enable Game Patching` and press A. It should now be `(x) Enable Game Patching`.
10. Press START to save and reboot.
11. Launch your mod!
