---
title: How to import YW3/B2 animation to YW2/B1
layout: default
parent: Models And Animations
grand_parent: Modding Guides
---

# How to import YW3/B2 animation to YW2/B1
**Original author on Discord: @zephixx**


## Required tools
Python

Metanoia

XMTNConverter

[This python script](https://cdn.discordapp.com/attachments/1218893271645028474/1218893481012101190/MINF2Converter-1.py?ex=66095203&is=65f6dd03&hm=ba107187afa65e75d0f1bd4b1ff7e107ac86c47661c35073d5ccb66ea4e10dce&)

## The Guide

1st part : Necessary file
Open your .fa and choose the yokai you want to convert animation (y....... Or x.......) then extract his p00,p10 and p20/p84. Inside the p10 and the p20/p84, there is a 000.mtninf2, extract it too.

2nd part : extract animation with Metanoia 
First, open metanoia, click on file at the top left, and click on open (open the p00 of your yokai), then click on open as model.  Then click on file and click on import animation (p10 or p20/p84) then click on export animation, a message will appear, click on yes.  Now choose a folder and extract the files.  Now you should have several files called "out_00".  You need to do this for both animation files at once, not at the same time.

3rd part :Transform out_00 into mtn2

In the files that you extracted with metanoia, delete all those that have Japanese next to them, keeping only out_00.  Then open xmtn converter, choose as animation name out_00, and then click on open smd and open your out_00. Then name the file he wants you to save 000 and now you have a 000.mtn2!
Dont forget that you have to do this for all the file (p10 and p20/p84)

Part 4: Recover mtninf files
Now click on this:
![image](https://cdn.discordapp.com/attachments/1218893271645028474/1218893722352222249/Screenshot_2024-03-14-22-17-00-966-edit_com.Discord-1.jpg?ex=6609523d&is=65f6dd3d&hm=a0f7c51579d57e6b6c32b66202e4e8cc5edef1d761d6a9f940dfdd4295ab0d6e&
)

delete the text and write cmd.  Now that you have the windows cmd open, paste this: MINF2Converter-1.py 000.mtninf2 output
And then check the output folder, there should be multiple mtninf files. Dont forget that you have to repeat the process for the 000.mtninf2 of the p10 and the 000.mtninf2 of the p20/84

5th part : Replace the file 

Now open the .fa of your game (Blasters or YW2) and extract a yokai file (xc) 
Create a folder called y...... (The folder must not be called like any other, it must be unique) and put all the the yokai file you extract inside it, then, import your yokai folder inside data/charaters.
Now that you have your folder, replace the p00 by your YW3/B2 model. Now double click on the p10, and replace the mtn2 by the one you create, and same for the mtninf. Same process for the p20/p84
Dont forget to rename the file, for exemple, if you extract y152000_p00.xc, rename the file by y152010_p00.xc.
Now test in game ! 
