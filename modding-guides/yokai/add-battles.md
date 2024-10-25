---
title: Add battles
layout: default
grand_parent: Modding Guides
parent: Yo-kai and battles
---

# How to add battles
**original guide by @kirasnuggets on discord**



Step 1. Extract Common_enc
(Located at data/res/battle)

Step 2. Open OPF's cfgbineditor and open Common_enc

Step 3. Duplicate an ENCOUNT_TABLE and ENCOUNT_CHARA to start with, in your duplicated Encount_Chara, paste your Yo-kai ParamID in the YokaiParamID slot

Step 4. In your Encount_Table, look for Offset1 and put in the number of your duplicated Encount_Chara, if theres more Offsets with Values put into them, set them to -1

Step 5. Before leaving your Encount Table, be sure to generate a new EncountID in the CRC-32 format
