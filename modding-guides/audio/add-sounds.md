---
title: Add Sounds
layout: default
grand_parent: Modding Guides
parent: Audio
---

# How to add sounds
**original guide by @whisperito on discord**



## Hello, in this guide, I will explain you how you can add sounds to the games

*Please note that this guide assumes that you already have the `.bcstm` sound you want to add to the game.*

# __Requirements__

- An extracted RomFS. If you don't have one.

# __For adding a music/theme to the `.bcsar`:__

1. Open your `.bcsar` in BCSARView, it should be in the `snd/folder` of the RomFS
2. In the "Create" menu, select "External Sound"
3. Another Window will open
     - In "Name", put the name of your `.bcstm` (remember, it's **case sensitive**!)
     - In "Path", put the path to your `.bcstm` (from the `.bcsar`'s directory) (e.g:       `stream/[bcstm_name.bcstm]`) 
     - In "Player", select "BGM"
     - In "Template", selecting any theme/music should work. 
4. Save your `.bcsar`. It should now work. Don't forget to put your `.bcstm` in the folder you specified in "Path"

# __For adding a  voice (medallium) to the `.bcsar`:__

1. Open your `.bcsar` in BCSARView, it should be in the `snd/folder` of the RomFS
2. In the "Create" menu, select "External Sound"
3. Another Window will open
     - In "Name", put the model ID of your Yo-kai (don't forget to add the `.bcstm` with the same name! (In YW1: it should be `yXXX` (4 first characters of the ModelID), for YW2 and further: `yXXXXXX` (Full ModelID.) )
     - In "Path", put the path to your `.bcstm` (from the `.bcsar`'s directory) (e.g:       `stream/[bcstm_name.bcstm]`) 
     - In "Player", select "VOICE"
     - In "Template", selecting any voice should work. 
4. Save your `.bcsar`. It should now work. Don't forget to put your `.bcstm` in the folder you specified in "Path"

***If it doesn't work, please tell me, like this, I will be able to fix the guide!***