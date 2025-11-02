---
title: Adding Custom QR Codes!! (YW2/YW3)
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# Adding Custom QR Codes
> **Disclaimer: This guide will roughly work on Yo-kai Watch 2 and 3, but not Yo-kai Watch 1 as this guide generates V2 QR Codes - it also wont teach you how to make random color coins as im lazy.**

## Yo-kai Watch 2
First, navigate over to `data/res/qr`. You should see 3 files (in this case `_*` means something such as `_0.04b`):
* `qr_config_ko_*.cfg.bin` - This is for KOR only QRs. **DO NOT USE THIS**.
* `qr_config_*.cfg.bin` - This is for V1 and V2 Codes. This is the file you want to edit.
* `qr_config.cfg.bin` - This is an old file. **DO NOT USE THIS**.

Once you've navigated to the file, open it in CfgBin Editor with the [latest mytags](../modding-resources/cfgbin-tags.html).
In `QR2_INFO_LIST` (NOT `QR1_INFO_LIST` as that tree is for V1 QR Codes; which won't be covered in this guide):
* Click on the tree/top-level `QR2_INFO_LIST` and increase the number by one.
* Go to the last entry (i.e. `QR2_INFO_441` on the latest version of Psychic Specters), right click it and click duplicate.
* Increase `StartTypeDec` to the previous entries `EndTypeDec` + 1, and `EndTypeDec` to the new `StartTypeDec` + 1.
* Then change the `ItemID` to whatever you want :P
* Finally, lets generate some QRs for distribution: Get the `StartTypeDec` and convert it to base36 using a website like [this](https://www.unitconverters.net/numbers/decimal-to-base-36.htm).
* Then Go to [QRTool](https://n123git.github.io/QRTool/) - and type the result under V2 type (Make sure the letters are capitalised!).
* Then select the amount of QRs to generate and enjoy!

## Yo-kai Watch 3
First, navigate over to `data/res/qr`. You should see 1 file:
* `qr_config_*.cfg.bin` - open it with CfgBin Editor.
Once you've navigated to the file, open it in CfgBin Editor with the [latest mytags](../modding-resources/cfgbin-tags.html).
In `QR2_INFO_LIST` (NOT `QR1_INFO_LIST` as that tree is for V1 QR Codes; which won't be covered in this guide):
* Click on the tree/top-level `QR2_INFO_LIST` and increase the number by one.
* Go to the last entry (i.e. `QR2_INFO_2227` on one version of Yo-kai Watch 3), right click it and click duplicate.
* Increase `StartTypeDec` to the previous entries `EndTypeDec` + 1, and `EndTypeDec` to the new `StartTypeDec` + 1.
* Then decide if you want the QR Code to give the player 1 or 2 items:
  * If you want 1 item, set the `Item2ID` to `0`.
* Configure the rest of the params as you wish, `Flag1ID` and `Flag2ID` are `GlobalBitFlag`s that will be activated (set to `1`) once the QR Code has been scanned.
* Set `RandomQRTable` to 0
* Finally, lets generate some QRs for distribution: Get the `StartTypeDec` and convert it to base36 using a website like [this](https://www.unitconverters.net/numbers/decimal-to-base-36.htm).
* Then Go to [QRTool](https://n123git.github.io/QRTool/) - and type the result under V2 type (Make sure the letters are capitalised!).
* Then select the amount of QRs to generate and enjoy!
