---
title: Adding Custom QR Codes!! (YW2)
layout: default
grand_parent: Modding Guides
parent: General Modding
---

> **Disclaimer: This guide will roughly work on later games such as Yo-kai Watch 3, but not Yo-kai Watch 1 as this guide generates V2 QR Codes - it also wont teach you how to make random color coins as im lazy.**

First, navigate over to `data/res/qr`. You should see 3 files:
* `qr_config.cfg.bin` - This is for V1 Codes (YW1 Compatibility). **DO NOT USE THIS**.
* `qr_config_ko_0.06c.cfg.bin` - This is for KOR only QRs. **DO NOT USE THIS**.
* `qr_config_0.04b.cfg.bin` - This is for V2 Codes. This is the file you want to edit.

Once you've navigated to the file, open it in CfgBin Editor with the [latest mytags](../modding-resources/cfgbin-tags.html).
In `QR2_INFO_LIST`:
* Click on the tree/top-level `QR2_INFO_LIST` and increase the number by one.
* Go to the last entry (`QR2_INFO_441` on the latest version of Psychic Specters), right click it and click duplicate.
* Increase `StartTypeDec` and `EndTypeDec` by 1 and change the `ItemID` to whatever you want :P
* Finally, lets generate some QRs for distribution: Get the `StartTypeDec` and convert it to base36 using a website like [this](https://www.unitconverters.net/numbers/decimal-to-base-36.htm).
* Then Go to [QRTool](https://n123git.github.io/QRTool/) - and type the result under V2 type (Make sure the letters are capitalised!).
* Then select the amount of QRs to generate and enjoy!
