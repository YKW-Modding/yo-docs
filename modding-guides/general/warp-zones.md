---
title: How to edit map warp zones in Yo-kai Watch 3 and 2
layout: default
grand_parent: Modding Guides
parent: General modding
---

# How to edit map warp zones in Yo-kai Watch 3 and 2
**original guide by @lit_8 on Discord**

Note: This only works on map and basic door warp zones, so no doors that require items, no elevators and no trains.
1- Open your map config folder, so it should be the file /data/map/yourmapid/mapenv.pck

2- Export that file and open it.

3- Inside that file it should be a file named "yourmapid_funcpt.bin" 

4- Export that file and edit it with Kuriimu (the text editing one)

5- Search a text string with (only) the map id of the map that that warp zone normally loads to.

6- Change the name of that text string to the map you want to loadmap.

7- Save the file and replace it with the original "yourmapid_funcpt.bin" 

8- Save the "yourmapid_funcpt.bin" file and replace it back to "mapenv.pck" 

9- Save "mapenv.pck" and replace it with the original to the map folder.

10- Save your yw_a.fa file and test it.
