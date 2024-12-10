---
title: Playing FA Mods (Legacy)
layout: default
grand_parent: Modding Guides
parent: Playing Mods
---

# Playing FA Mods (Legacy)

### Prerequisites

If the mod is just a .FA file (Maybe multiple, or maybe with folders called `mov` or `snd`), this guide is for you. Take all of the files and put them inside a folder called `romfs`.
Since Yo-kai Watch 1 for Nintendo Switch also uses .FA, a guide for it will be listed here.

## Citra

This is quite simple!

1. Ok, so you have your mod files in your `romfs` folder. Copy the folder.
2. Open Citra and right click the game you are using. Select "Open Mods Directory".
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
3. Go to `luma/titles`.
4. Go to [3dsdb](http://3dsdb.com) and search the title of your game to find its Title ID.
5. In `luma/titles` make a folder and name it the Title ID.
6. Copy your `romfs` folder and paste it in the folder you just made. You can now reinsert your SD card into the console.
7. Hold SELECT while booting your console.
8. Use the DPad to navigate down to `( ) Enable Game Patching` and press A. It should now be `(x) Enable Game Patching`.
9. Press START to save and reboot.
10. Launch your mod!
