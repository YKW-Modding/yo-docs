---
title: How to add wild Yokai spawns
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---
# How to add wild Yokai spawns
**Original guide by Coolrils on Discord**

This guide will show you how to add spawns to yo-kai, any questions feel free to ping me, Im always happy to help.

What you will need: 

Kuriimu 2: this lets you open the fa files (https://github.com/FanTranslatorsInternational/Kuriimu2/releases/tag/1.2.1) download the gui version.

CfgBinEditor: This is what you will use to actually edit the files (https://github.com/onepiecefreak3/CfgBinEditor/releases/tag/1.2.2) .

MyTags: These will let you see more clearly what each value is (https://raw.githubusercontent.com/light8227/ykw-stuff/refs/heads/master/MyTags.txt) simply press ctrl+s while on the page to download, and do not change the name of the file.

Albatross: This is a database that has a ton of useful Info for modding (https://github.com/Tiniifan/Albatross/releases/tag/albatross-new).

Yo kai watch 2 dump: You can do this by going into citra and right clicking your yo-kai watch save and clicking dump.

The actual process!

1. First you need to find the map you intend for the yo kai to spawn in, to do this go to this website and find the id of your map (https://yokai.wiki/modding-resources/map-ids.html).

2. Next open your Kuriimu2 (if it doesnt open make sure you have the .net installed from kuriimu's github, if you need help with that just ask!). With kuriimu,
navigate to your games dump and open your `yw[X]_a.fa` (`X` represents your games number, it may also just be `yw_a.fa`) Then in `yw[X]_a.fa` navigate to `data/res/map/[MAPID] `

3. Click on your MapID and there should be a file called `[MAPID].pck`. double click it to open and extract the file `[MAPID]_enc_0.03a.cfg.bin` ( `0.03a` could be named something else)

4. Open CfgBin Editor and ensure The MyTags file is in the same folder as CfgBinEditor exe. then open `[MAPID]_enc_0.03a.cfg.bin` with CfgBinEditor (next to the big red X there should be a little box click that box and select the game you are modding, this is the MyTags) after opening your file, you should see `ENCOUNT_CHARA_BEGIN_0`  open that, all the entries here are yo kai spawns, if you enabled MyTags correctly you should see the ParamIds of the yo kai at the top of the entrys. (ParamId is What yo kai it is)

5. Now navigate to Albatross and open it (```Albatross-main\Albatross\bin\Release````)  click new, select your game, language and choose your games dump folder as your Extracted Romfs path. Once done click on your game and choose Charaparam, Now simply search the yo kai you want and copy their ParamId (Hash).

6. Now that you have the yo kais ParamId, Go back to `ENCOUNT_CHARA_BEGIN_0` and find a Yo kai's entry and replace one or more of their ParamId with the yo kai you are trying to spawns ParamId.

7. Now save your `[MAPID]_enc_0.03a.cfg.bin` and go back to kuriimu2 and navigate to `[MAPID]_enc_0.03a.cfg.bin` once again, replace the first `[MAPID]_enc_0.03a.cfg.bin` with the one with just edited, then save your `yw[X]_a.fa` file and put it in your games mod folder

8. The Yo kai should now be spawning in the place of the yo kai you replaced, if not troubleshoot with someone.

That should mostly be it, if you have any additonal questions Ping me or ask around on the diwscord. If im offline Im just invisble lol.


Credits to Whisperito, His Post about the subject showed me what to do, and I only expanded upon his guide