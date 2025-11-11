---
title: Getting Started
layout: default
parent: Modding Guides
has_children: true
nav_order: 0

---

# Getting Started
This guide aims to help you get started modding 3DS Yo-kai Watch games.

## Dumping RomFS and Extracting Main Archives
### Dumping RomFS
First you will need to Dump RomFS for the game you'll want to mod.
You can do this using an emulator or a modded 3DS; this guide will cover both:

On 3DS you can do so by following [this](https://gist.github.com/PixelSergey/73d0a4bc1437dbaa53a1d1ce849fdda1) guide under the "Extracting RomFS from a 3DS game cartrige" or "Extracting RomFS from an installed title" sections depending on your game source.
On Citra, Azahar (formerly Lime3DS) or Azahar this can be done by right clicking an installed title and selecting "Dump RomFS".

### Extracting Main Archives
Most of the game's data (aside from alot of audio) is packed in main game and language archives; think of these as `.zip`s.
Now you can work with an unextracted `.fa`; especially if you don't have alot of storage, but doing this will make your life alot easier.

Download [Kuriimu2](https://github.com/FanTranslatorsInternational/Kuriimu2/releases/latest) and then open it; on some computers it takes a bit to open.
Then click on File > Open (or Ctrl+O) and select the main game `fa` depending on the game this will either be:
* `yw_a.fa` -> yw3, ywb, ywb2
* `yw2_a.fa` -> yw2
* `yw1_a.fa` -> yw1
Next, on the left hand side you'll see a sidebar with the `fa`'s name. Right click that section, click Extract and then save to wherever you want.
Finally, enjoy!

## Using CfgBin Editor
First, download the latest version of opf's `CfgBin Editor` this is a tool you will use very frequently so this tutorial will cover it; you can download it [here](https://github.com/onepiecefreak3/CfgBinEditor/releases/latest).

Then download and set up the latest `MyTags.txt`, you can do so using [this](modding-resources/cfgbin-tags.html) short guide.
> If CfgBin Editor is already open, you'll have to reopen it for the changes to apply.

Next open `CfgBin Editor` for the next steps:
* Click on [this](/item_config_0.05d.cfg.bin) link and click the download button to download this example `cfg.bin` (to be specific it's YW1 EUR's `item_config`.)
* Optionally click on Settings > Languages and Settings > Themes to adjust the tool to your liking.
* Then, open the file in `CfgBin Editor` using File > Open (or the shortcut Ctrl+O)
You should then see something like this:
![Screenshot of cfg.bin editor](assets/mainShot2.png)
Now lets practice a few edits!
First and most importantly of all, let's activate MyTags; this file is from Yo-kai Watch 1 so select YW1 under the MyTags select.

You should see something like this:

![Screenshot of cfg.bin editor](assets/MyTagsSelect.png)
* Click "Add Root Entry" (under the search bar)
* Now type the name of the entry you wish to create, for this example i'll type "hello!"
* Click "Ok". You should end up with something like this:

![Screenshot of cfg.bin editor](assets/newRootEntry.png)
If something is green, in this case the root entry/tree it means it's unsaved. You can click the save button to save the change.
Additionally, if an error occurs (i.e. you try to open something that isn't a `cfg.bin`) in this case before screenshotting I made a root entry without a name.

Next you can right click an entry to see a list of buttons, I wont cover them as they are very self-explanatory, but I will show a picture!
![Screenshot of cfg.bin editor](assets/entryControls.png)
