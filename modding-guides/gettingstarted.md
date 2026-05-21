---
title: Getting Started
layout: default
parent: Modding Guides
has_children: true
nav_order: 0

---

# Getting Started
This guide should help you get started modding 3DS Yo-kai Watch games. I have decided to cover three steps, in this guide: dumping and extracting the game's RomFS, learning to use K2 and CfgBin Editor, and understanding how to generate IDs. After which you'll be ready to follow a specific modding guide.

## Dumping RomFS and Extracting Main Archives
### Dumping RomFS
First you will need to dump RomFS for the game you want to mod.
You can do this using an emulator or a modded 3DS; this guide will cover both:

On 3DS you can do so by following [this](https://gist.github.com/PixelSergey/73d0a4bc1437dbaa53a1d1ce849fdda1) guide under the "Extracting RomFS from a 3DS game cartridge" or "Extracting RomFS from an installed title" sections depending on your game source.
On Citra, Azahar (formerly Lime3DS) or AzaharPlus this can be done by right clicking an installed title and selecting "Dump RomFS".

### Extracting Main Archives
Most game data, including textures and configs but excluding most audio, is packed in main game and language archives — think of these as `.zip`s.
While it's possible to work directly with the `.fa` archive without extracting it (especially if you don't have a lot of storage) extracting it will make your life a lot easier.

Download [Kuriimu2](https://github.com/FanTranslatorsInternational/Kuriimu2/releases/latest) and then open it; on some computers it takes a bit to open.
Then click on File > Open (or Ctrl+O) and select the main game `fa`. Depending on the game, this file is located in the root of your dumped RomFS and will be named:
* `yw_a.fa` -> yw3, ywb, ywb2
* `yw2_a.fa` -> yw2
* `yw1_a.fa` -> yw1

Next, on the left hand side you'll see a sidebar with the `fa`'s name. Right click that section, click Extract and then save to wherever you want.
Finally, enjoy!

## Using CfgBin Editor
First, download the latest version of opf's `CfgBin Editor` - a tool you'll use frequently throughout your time modding these games. You can download it [here](https://github.com/onepiecefreak3/CfgBinEditor/releases/latest).

Then download and set up the latest `MyTags.txt`, you can do so using [this](modding-resources/cfgbin-tags.html) short guide.
> If CfgBin Editor is already open, you'll have to reopen it for the changes to apply.

Next open `CfgBin Editor` for the next steps:
* Click on [this](/item_config_0.05d.cfg.bin) link and click the download button to download this example `cfg.bin` (to be specific it's YW1 EUR's `item_config`.)
* Optionally click on Settings > Languages and Settings > Themes to adjust the tool to your liking.
* Then, open the file in `CfgBin Editor` using File > Open (or the shortcut Ctrl+O)
You should then see something like this:
![Screenshot of cfg.bin editor](assets/mainShot2.png)
Now let's practice a few edits!
First and most importantly of all, let's activate MyTags; this file is from Yo-kai Watch 1 so select YW1 under the MyTags select.

You should see something like this:

![Screenshot of cfg.bin editor](assets/MyTagsSelect.png)
* Click "Add Root Entry" (under the search bar)
* Now type the name of the entry you wish to create, for this example I'll type "hello!"
* Click "Ok". You should end up with something like this:

![Screenshot of cfg.bin editor](assets/newRootEntry.png)
If you see a green entry (in this case the root entry/tree), then the entry in question is unsaved. You can click the save button to save the change.
Additionally, if an error occurs (i.e. you try to open something that isn't a `cfg.bin`) you'll see somethng on the bottom left of the UI. In this case, before I took the screenshot, I made a root entry without a name.

Next you can right click an entry to see a list of buttons. I won't cover them as they are very self-explanatory, but I will show a picture!
![Screenshot of cfg.bin editor](assets/entryControls.png)

Now let's do something practical — adding a new entry to `ITEM_INDEX`:
* Click on the `ITEM_INDEX` root entry
* Increase `ChildCount` by 1
* Now we are going to add a child to the tree
* Click on the arrow to expand the tree
* Scroll down to the last entry (`ITEM_INDEX_380` meaning the 381st `ITEM_INDEX`).
* Right click it and click Duplicate
* Now click on the newly created `ITEM_INDEX` we are going to edit the values/params
* Increase the `MainItemIndex` by 1.

## Generating Generic IDs
A lot of guides will ask you to generate an ID, i.e. `ItemID`s.
You can do this by clicking the randomise button shown below:
![Screenshot of cfg.bin editor](assets/randomize_icon.png)
However, for some IDs a specific structure is required — e.g. `cpsl_text_<model>` — for these follow the section below.

## Generating CRC-32 hashes
First we need the specific pattern; for this example we'll use capsule dialogue which follows the pattern `cpsl_text_<model>`. We will first substitute the values, in this case `<model>`, this stage is self explanatory and depends on the pattern in question, but after substitution, you should have something similar to: `cpsl_text_y101000`.
Next, we need to hash the text. You don't need to know the specifics of what it does, but what you should know is that it takes the text as an input and returns an ID. Go to a website such as [this](https://emn178.github.io/online-tools/crc/) and type your text, then copy the output.  You should end up with something like `940fdaec`. In `CfgBin Editor`, you should add a `0x` prefix to the beginning, giving you `0x940fdaec`.

You're now ready to follow a specific modding guide, check the sidebar for whichever area you'd like to mod!
