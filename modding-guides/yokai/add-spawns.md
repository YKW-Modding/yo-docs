---
title: Adding Yo-kai Spawns
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---
# Adding Yo-kai Spawns (Dungeon and Search Points)

**Guide by Light8227**

This guide will show you how to add spawns for a Yo-kai in YW2. The process is slightly different for dungeons and search points, but there are a lot of same points. Dungeons will be covered first.

## Dungeons ([MapID]_enc_pos.cfg.bin)

If you'd like to add your spawn to a dungeon, follow this section. If not, ignore this and continue to the Search Points section.

1. First, find the map you want to edit the spawns of, and open the `[MapID].pck` file in Kuriimu2. Find the file called `[MapID]_enc_pos.cfg.bin`, extract it, and open it in CfgBinEditor. Then, enable the YW2 tags and expand the SET_PATH_POP_LIST trees. You can see that it references an EncountID, making it pretty simple. Duplicate an entry, input your own EncountID, and then click on the tree itself to increase the ChildCount. If you aren't sure which ones you want to edit, you could either edit all of the trees, or just the ones with a certain group of Yo-kai. (For example, you could only add on to the trees with EncountIDs for Smogling, if you so desire.)

2. Head back to the `.pck` in Kuriimu2 again, and replace the `enc` and `enc_pos` files with the ones you edited, and save the `.pck`.

After that, you're done! You might need to refresh the area a few times to see your spawn, but it's that simple.

## Search Points ([MapID]_watchmap_0.02c.cfg.bin)

If you'd like to add your spawn to a search point, follow this section. If not, ignore this and go back up to the Dungeons section.

1. Head back to the `.pck` in Kuriimu2, find the file called `[MapID]_watchmap_0.02c.cfg.bin`, extract it, and open it in CfgBinEditor. Like before, enable the YW2 tags. Now, there are three trees we want to look at: WM_YOKAI_WATCHMAP specifies which entries to look at when putting Yo-kai at a search point. WM_YOKAI_APPEAR specify the Yo-kai showing during the search minigame, the chances for it to be used, and which encounter(s) to set for when the battle starts. WM_YOKAI_ENCOUNT specifies the EncountIDs utilized and the chance for them to be used.

2. We'll start with WM_YOKAI_ENCOUNT. Duplicate an entry and set the EncountID to the one you want. If you have multiple encounters you want it to pull from, such as a battle with three Yo-kai and a battle with one Yo-kai, duplicate more entries and set the EncountIDs and Probability accordingly. Then click on the tree itself to increase the ChildCount.

3. In WM_YOKAI_APPEAR, duplicate an entry accordingly and set the BaseID to the Yo-kai you're adding a spawn for. For WMYokaiEncountStartPos, set it to the first entry it will call in WM_YOKAI_ENCOUNT. (For example, if it's set to 2, it will start looking at WM_YOKAI_ENCOUNT_2 to call first.) WMYokaiEncountLength will depend on how many encounters you have for this Yo-kai. (For this example, I'll assume it's two. So, if you set the StartPos to 2 and tthe Length to 2, it will call WM_YOKAI_ENCOUNT_2 and WM_YOKAI_ENCOUNT_3. Essentially, the number is how many entries it will call, including the one you set as the StartPos.) The Cond is a CExpression you can use to limit when the spawn can appear outside of normal conditions if you so desire. The Weight variables are likely the chances for this Yo-kai to appear at a search point, although personally, I couldn't get them to spawn unless the weight between Yo-kai at that spawn was equal to or less than 100. (For example, having 6 Yo-kai with weights of 25 didn't work for me, but 6 Yo-kai with weights of 19 did.) Then click on the tree itself to increase the ChildCount.

4. Once you have an edited WM_YOKAI_APPEAR entry, we're going to have to do some annoying stuff. The way different spawn points call various Yo-kai is similar to how APPEAR entries call ENCOUNT entries. However, unlike those, I couldn't manage to get the map to successfully call new entries at the correct spawn points with the entries sitting at the end. So first, identify which spawn point is which--This can be easily done by seeing what BaseIDs are in WM_YOKAI_APPEAR, and seeing in what range they fall in WM_YOKAI_WATCHMAP. Then right-click on your modded entry, and select Export. Give it whatever name you want, as long as it ends in `.json`. Find the range of the spawn point you want, right-click an entry there, and press Import, and select your `.json`. It should add an entry right after the one you right-clicked with all the data intact. After doing so, you can delete the identical entry at the bottom of the tree.

5. In WM_YOKAI_WATCHMAP, identify the entry for the spawn point like earlier, and just edit the ranges to include your new entry. Make sure to edit the entries after the one you edit accordingly. (For example, if I only edited the spawn point for WM_YOKAI_WATCHMAP_1 to have a Length of 6 instead of 5, then I would need to edit all the entries after that to have a higher StartPos by 1, so that they all still call the same Yo-kai.)

6. Head back to the `.pck` in Kuriimu2 again, and replace the `enc` and `watchmap` files with the ones you edited, and save the `.pck`.

## Search Points Contd. (watchmap_common_0.03f.cfg.bin)

1. Go to `data/res/map`, scroll all the way to the bottom, and locate `watchmap_common_0.03f.cfg.bin`. This file specifies which Yo-kai actually show up when you try to search at a search point. If this isn't set up properly, your radar will go nuts but you'll get a message that nothing is there when you try investigating. Open the file in CfgBinEditor, and enable the YW2 tags. Duplicate an entry in WM_YOKAI_INFO, and change the BaseID to that of the Yo-kai whose spawn point you're adding.

2. This next part is a little tricky, as it's not as well documented, so you may need some trial and error. If the entries after the BaseID are set up wrong, then the game will do what was previously mentioned, with no Yo-kai showing up during the search minigame. I would recommend finding the Yo-kai you duplicated for the `watchmap` file and copying the values from their entry in this file. It should work, but if it doesn't, experiment around some more until it does. (Reminder: You can set which Yo-kai to call for a search point in the `watchmap` file, if you want to do some specialized testing on your new Yo-kai. If you do this, just make sure to edit it again to include the rest of the Yo-kai at that spawn point!) Click on the tree itself afterwards and increase the ChildCount.

After all that, your Yo-kai should *finally* show up at search points...with enough luck, that is.
