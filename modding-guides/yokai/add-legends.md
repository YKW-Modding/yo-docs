---
title: How to Add Legendary Yo-kai
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---
# How to Add Legendary Yo-kai
**Original guide by @stringsbutalt on Discord**

For this you will need CfgBinEditor & MyTags.

1. Open CfgBinEditor, find res/legend/legend_config.cfg.bin

2. Copy a entry from LEGEND_DATA_CONFIG

(Make sure to select YW2 on the tag options YW1 doesn't have it labeled)

3. Make a new LegendID, use this website https://emn178.github.io/online-tools/crc32.html, type whatever you want for the input and copy the Hex Upper Case output into the LegendID value

4. Get the ParamID of the legendary yokai (yokai you will get from the 8 seals) and copy it into "LegendYokaiParamID"

5. You should see Seal1YokaiParamID, Seal2YokaiParamID, etc. Set the ParamID of your yokai to the 8 yokai seals.

6. You can also make it so you can tell if the yokai you need for the seal in the "SpoilSeal" box. Set it 0 to not spoil and 1 to spoil what yokai you need.

7. Add 1 to the value of LEGEND_DATA_CONFIG.

![SS](https://i.imgur.com/SKaGmp2.png)
