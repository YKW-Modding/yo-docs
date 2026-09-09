---
title: Adding Yo-kai Spawns
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---
# Adding Yo-kai Spawns (Dungeon and Search Points)

**Guide by Light8227**
This guide will show you how to add spawns for a Yo-kai in YW2. The process is slightly different for dungeons and search points, but there are a lot of same points. Dungeons will be covered first.

What you will need: 
Required tools:

Kuriimu 2: this lets you open the fa files (https://github.com/FanTranslatorsInternational/Kuriimu2/releases/tag/1.2.1) download the gui version.
[Kuriimu2](https://github.com/FanTranslatorsInternational/Kuriimu2-ImGuiForms-Update/releases): Needed for extracting files from the `.fa` and `.pck` files.

CfgBinEditor: This is what you will use to actually edit the files (https://github.com/onepiecefreak3/CfgBinEditor/releases/tag/1.2.2) .
[CfgBinEditor](https://github.com/onepiecefreak3/CfgBinEditor/releases): Needed for opening and editing `.cfg.bin` files.

MyTags: These will let you see more clearly what each value is (https://raw.githubusercontent.com/light8227/ykw-stuff/refs/heads/master/MyTags.txt) simply press ctrl+s while on the page to download, and do not change the name of the file.
## Dungeons ([MapID]_enc_pos.cfg.bin)

Albatross: This is a database that has a ton of useful Info for modding (https://github.com/Tiniifan/Albatross/releases/tag/albatross-new).
If you'd like to add your spawn to a dungeon, follow this section. If not, ignore this and continue to the Search Points section.

Yo kai watch 2 dump: You can do this by going into citra and right clicking your yo-kai watch save and clicking dump.
1. First, find the map you want to edit the spawns of, and open the `[MapID].pck` file in Kuriimu2. Find the file called `[MapID]_enc_pos.cfg.bin`, extract it, and open it in CfgBinEditor. Then, enable the YW2 tags and expand the SET_PATH_POP_LIST trees. You can see that it references an EncountID, making it pretty simple. Duplicate an entry, input your own EncountID, and then click on the tree itself to increase the ChildCount. If you aren't sure which ones you want to edit, you could either edit all of the trees, or just the ones with a certain group of Yo-kai. (For example, you could only add on to the trees with EncountIDs for Smogling, if you so desire.)

The actual process!
2. Head back to the `.pck` in Kuriimu2 again, and replace the `enc` and `enc_pos` files with the ones you edited, and save the `.pck`.

1. First you need to find the map you intend for the yo kai to spawn in, to do this go to this website and find the id of your map (https://yokai.wiki/modding-resources/map-ids.html).
After that, you're done! You might need to refresh the area a few times to see your spawn, but it's that simple.

2. Next open your Kuriimu2 (if it doesnt open make sure you have the .net installed from kuriimu's github, if you need help with that just ask!). With kuriimu,
navigate to your games dump and open your `yw[X]_a.fa` (`X` represents your games number, it may also just be `yw_a.fa`) Then in `yw[X]_a.fa` navigate to `data/res/map/[MAPID] `
## Search Points ([MapID]_watchmap_0.02c.cfg.bin)

3. Click on your MapID and there should be a file called `[MAPID].pck`. double click it to open and extract the file `[MAPID]_enc_0.03a.cfg.bin` ( `0.03a` could be named something else)
If you'd like to add your spawn to a search point, follow this section. If not, ignore this and go back up to the Dungeons section.

4. Open CfgBin Editor and ensure The MyTags file is in the same folder as CfgBinEditor exe. then open `[MAPID]_enc_0.03a.cfg.bin` with CfgBinEditor (next to the big red X there should be a little box click that box and select the game you are modding, this is the MyTags) after opening your file, you should see `ENCOUNT_CHARA_BEGIN_0`  open that, all the entries here are yo kai spawns, if you enabled MyTags correctly you should see the ParamIds of the yo kai at the top of the entrys. (ParamId is What yo kai it is)
1. Head back to the `.pck` in Kuriimu2, find the file called `[MapID]_watchmap_0.02c.cfg.bin`, extract it, and open it in CfgBinEditor. Like before, enable the YW2 tags. Now, there are three trees we want to look at: WM_YOKAI_WATCHMAP specifies which entries to look at when putting Yo-kai at a search point. WM_YOKAI_APPEAR specify the Yo-kai showing during the search minigame, the chances for it to be used, and which encounter(s) to set for when the battle starts. WM_YOKAI_ENCOUNT specifies the EncountIDs utilized and the chance for them to be used.

5. Now navigate to Albatross and open it (```Albatross-main\Albatross\bin\Release````)  click new, select your game, language and choose your games dump folder as your Extracted Romfs path. Once done click on your game and choose Charaparam, Now simply search the yo kai you want and copy their ParamId (Hash).
2. We'll start with WM_YOKAI_ENCOUNT. Duplicate an entry and set the EncountID to the one you want. If you have multiple encounters you want it to pull from, such as a battle with three Yo-kai and a battle with one Yo-kai, duplicate more entries and set the EncountIDs and Probability accordingly. Then click on the tree itself to increase the ChildCount.

6. Now that you have the yo kais ParamId, Go back to `ENCOUNT_CHARA_BEGIN_0` and find a Yo kai's entry and replace one or more of their ParamId with the yo kai you are trying to spawns ParamId.
3. In WM_YOKAI_APPEAR, duplicate an entry accordingly and set the BaseID to the Yo-kai you're adding a spawn for. For WMYokaiEncountStartPos, set it to the first entry it will call in WM_YOKAI_ENCOUNT. (For example, if it's set to 2, it will start looking at WM_YOKAI_ENCOUNT_2 to call first.) WMYokaiEncountLength will depend on how many encounters you have for this Yo-kai. (For this example, I'll assume it's two. So, if you set the StartPos to 2 and tthe Length to 2, it will call WM_YOKAI_ENCOUNT_2 and WM_YOKAI_ENCOUNT_3. Essentially, the number is how many entries it will call, including the one you set as the StartPos.) The Cond is a CExpression you can use to limit when the spawn can appear outside of normal conditions if you so desire. The Weight variables are likely the chances for this Yo-kai to appear at a search point, although personally, I couldn't get them to spawn unless the weight between Yo-kai at that spawn was equal to or less than 100. (For example, having 6 Yo-kai with weights of 25 didn't work for me, but 6 Yo-kai with weights of 19 did.) Then click on the tree itself to increase the ChildCount.

7. Now save your `[MAPID]_enc_0.03a.cfg.bin` and go back to kuriimu2 and navigate to `[MAPID]_enc_0.03a.cfg.bin` once again, replace the first `[MAPID]_enc_0.03a.cfg.bin` with the one with just edited, then save your `yw[X]_a.fa` file and put it in your games mod folder
4. Once you have an edited WM_YOKAI_APPEAR entry, we're going to have to do some annoying stuff. The way different spawn points call various Yo-kai is similar to how APPEAR entries call ENCOUNT entries. However, unlike those, I couldn't manage to get the map to successfully call new entries at the correct spawn points with the entries sitting at the end. So first, identify which spawn point is which--This can be easily done by seeing what BaseIDs are in WM_YOKAI_APPEAR, and seeing in what range they fall in WM_YOKAI_WATCHMAP. Then right-click on your modded entry, and select Export. Give it whatever name you want, as long as it ends in `.json`. Find the range of the spawn point you want, right-click an entry there, and press Import, and select your `.json`. It should add an entry right after the one you right-clicked with all the data intact. After doing so, you can delete the identical entry at the bottom of the tree.

8. The Yo kai should now be spawning in the place of the yo kai you replaced, if not troubleshoot with someone.
5. In WM_YOKAI_WATCHMAP, identify the entry for the spawn point like earlier, and just edit the ranges to include your new entry. Make sure to edit the entries after the one you edit accordingly. (For example, if I only edited the spawn point for WM_YOKAI_WATCHMAP_1 to have a Length of 6 instead of 5, then I would need to edit all the entries after that to have a higher StartPos by 1, so that they all still call the same Yo-kai.)

That should mostly be it, if you have any additonal questions Ping me or ask around on the diwscord. If im offline Im just invisble lol.
6. Head back to the `.pck` in Kuriimu2 again, and replace the `enc` and `watchmap` files with the ones you edited, and save the `.pck`.

## Search Points Contd. (watchmap_common_0.03f.cfg.bin)

Credits to Whisperito, His Post about the subject showed me what to do, and I only expanded upon his guide
1. Go to `data/res/map`, scroll all the way to the bottom, and locate `watchmap_common_0.03f.cfg.bin`. This file specifies which Yo-kai actually show up when you try to search at a search point. If this isn't set up properly, your radar will go nuts but you'll get a message that nothing is there when you try investigating. Open the file in CfgBinEditor, and enable the YW2 tags. Duplicate an entry in WM_YOKAI_INFO, and change the BaseID to that of the Yo-kai whose spawn point you're adding.

2. This next part is a little tricky, as it's not as well documented, so you may need some trial and error. If the entries after the BaseID are set up wrong, then the game will do what was previously mentioned, with no Yo-kai showing up during the search minigame. I would recommend finding the Yo-kai you duplicated for the `watchmap` file and copying the values from their entry in this file. It should work, but if it doesn't, experiment around some more until it does. (Reminder: You can set which Yo-kai to call for a search point in the `watchmap` file, if you want to do some specialized testing on your new Yo-kai. If you do this, just make sure to edit it again to include the rest of the Yo-kai at that spawn point!) Click on the tree itself afterwards and increase the ChildCount.

After all that, your Yo-kai should *finally* show up at search points...with enough luck, that is.
