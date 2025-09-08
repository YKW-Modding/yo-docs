---
title: Editing Passwords in YO-KAI WATCH 4
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# Editing Passwords in YO-KAI WATCH 4
## Requirements
- [CfgBinEditor](https://github.com/onepiecefreak3/CfgBinEditor)
- [MyTags](https://github.com/light8227/ykw-stuff)
- [Viola](https://github.com/SuperTavor/Viola/releases)
- [CRC Online Tool](https://emn178.github.io/online-tools/crc/)

   To start, you'll need a dump of YO-KAI WATCH 4's unpacked files. Note that in this game, passwords are a bit more secure than just sitting in a text file for an easy edit, so the process is a bit more involved.
1.   You can find the file for passwords in `data/common/gamedata/post/password_list_config.cfg.bin`.
2.   Open the file in CfgBinEditor with the latest MyTags, click the dropdown near the top that says "None," and click "YW4." This will give you our researched variable names so you know what you're editing.
3.   When it comes to the variables you probably want to take note, that would be Password, ParamID, and NumberReceived. The Password variable is pretty self-explanatory—It's just not in an easily-readable format. The passwords are stored in CRC32 hashes. This will be covered soon. THe ParamID and NumberReceived correlate to the password's reward. It's also pretty self-explanatory, so if you want to change the items you get, change the ParamID to whatever item you want, and how many you want.
4.   Now for the actual password editing. The thing is, it's practically impossible to decode a password's hashes without knowing what it is already. There's just too many potential combinations for what it could be. However, we do know some of the released passwords and which entries they correlate to. Entries 2, 20, 24, 25, 27, 44, and 48 all have publicly released passwords. The others are unknown.
5.   Fortunately, making a custom password is really simple. If the Password variable in CFGBinEditor is not displaying as Hex already, check the box to make it do so. Then, go to the CRC Online Tool's website. Just in case it doesn't default to the right settings, I'll share them here. Input Type: Text, Input Encoding: UTF-8, Output Encoding: Hex (Upper Case), Model: CRC-32/ISO-HDLC (CRC-32, CRC-32/ADCCP, CRC-32/V-42, CRC-32/XZ, PKZIP). In the Input box, out whatever you want for your password. Keep in mind that it must be within YO-KAI WATCH 4's character limits, otherwise you obviously wouldn't be able to properly input it ingame. Afterwards, it'll give you the CRC32 password in hex and you can just put in the file.

Once you've edited to your heart's content, (Make sure to save!) there's only one thing left to do: Prepare a romfs directory and pack it.
1.   First, put your modded files in a mod directory with the same filepath as their location in the CPK file. For example, here, you would set up your directory as `/data/common/text/ja` and place the modded file there.
2.   You'll now want to pack your mod. Select the Pack button, and you'll be asked about using an external, vanilla cpk_list. Select Yes, and a window will open that lets you direct Viola to it. In your RomFS dump of the game, the cpk_list.cfg.bin file is in the `data` folder. I personally copied this to the root of my Viola folder to save time searching for it, as well as renaming it to YW4cpk_list.cfg.bin so that I can keep other games' cpk_lists in there.
3.   After that's done, you'll be asked for the target platform, which will be Switch. You'll then be asked for the input path. Select the folder with all your new mod files that you made earlier. For example, if this was the English Mod, I'd select `F:\Mods\Viola 1.3\YW4 EN Translation\romfs` as my path.
4.   Next, you'll have to select your output path. I like to set this directly to where Ryujinx, for example, loads mods so I don't have to worry about copying them over from my hard drive or anything.
