---
title: AYD to OBJ
layout: default
parent: Models and Animations
grand_parent: Modding Guides
---
# Converting Ayd Files to Obj

**Original guide by @web.83 on discord**

Hello, I will explain how to convert an Ayd (File from Puni Puni) to Obj (Usable for porting)
__Tools You will Need:__
https://github.com/simontime/fictional-couscous/releases/latest
https://github.com/Tiniifan/YKWPPModelConverter/releases/latest (Windows GUI is recommended)
__Step 1__
Download your ayd file(s) from the Puni Puni Dump https://mega.nz/folder/V2w0wZTT#znYzijCJFkXU6yjih4_41w

__Step 2__
Make sure the "Show file extension" option is checked in view (in the file explorer) and rename your ``[file].ayd`` to ``[file].zip``

__Step 3__
Extract the ez files from your zip files

__Step 4__
Open ``yokai.exe`` and open the .ez files

__Step 5__
Another zip files will appear, it contains the ymd files as well as the texture, voice etc

__Step 6__
Now create a folder named ``ymd`` next to YWPPConverter and put your ymd files and create another folder named ``obj`` 

__Final step__
Click convert after setting the folders paths in your GUI and your converted model will be in the ``obj`` folder!
