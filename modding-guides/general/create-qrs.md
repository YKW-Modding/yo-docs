---
title: Adding Custom QR Codes!!
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# Adding Custom QR Codes
> Guide was written by @n123original on discord. **Disclaimer: This guide will not teach you how to make random color coins as im lazy.** This guide assumes you already know how to navigate romfs and use CfgBin Editor. If not, please read [the starting guide](../gettingstarted.html).

This guide explains how to add custom QR Codes to Yo-kai Watch games by editing the QR Code config files. It has been confirmed to work for all 3DS Yo-kai Watch games aside from Sangokushi.

## Yo-kai Watch 1
First, navigate over to `data/res/qr`. You should see the following 2 types of files:
* `qr_config_ko*.cfg.bin` - This is for KOR only QRs. **DO NOT USE THIS UNLESS INTENTIONAL**.
  * The `*` refers to versioning, so instead of just `qr_config_ko.cfg.bin` you might also see files such as `qr_config_ko_0.06c.cfg.bin`. Pick the one with the highest version.
* `qr_config*.cfg.bin` - This is for V1 QR Codes. This is the file you want to use.
  * The `*` refers to versioning, so instead of just `qr_config.cfg.bin` you might also see files such as `qr_config_0.04b.cfg.bin`. Pick the one with the highest version.
 
Once you've navigated to the file, open it in CfgBin Editor with the [latest mytags](../../modding-resources/cfgbin-tags.html).
In `QR_INFO_LIST`:
* Click on the tree/top-level `QR_INFO_LIST` and increase the number by one.
* Go to the last entry (e.g. `QR_INFO_121`), right click it and click duplicate.
* Increase `StartTypeDec` to the previous entries `EndTypeDec` + 1, and `EndTypeDec` to the new `StartTypeDec` + 1.
* Then change the `ItemID` to whatever you want :P
* Finally, lets generate some QRs for distribution: Get the `StartTypeDec` and convert it to base36 using a website like [this](https://www.unitconverters.net/numbers/decimal-to-base-36.htm).
* Then open [QRTool](https://n123git.github.io/QRTool/) - select V1 and then type the result under "Type" (Make sure the letters are capitalised!).
* Then select the amount of QRs to generate and enjoy!

## Yo-kai Watch 2
First, navigate over to `data/res/qr`. You should see the following 2 types of files:
* `qr_config_ko*.cfg.bin` - This is for KOR only QRs. **DO NOT USE THIS UNLESS INTENTIONAL**.
  * The `*` refers to versioning, so instead of just `qr_config_ko.cfg.bin` you might also see files such as `qr_config_ko_0.06c.cfg.bin`. Pick the one with the highest version.
* `qr_config*.cfg.bin` - This is for V1 and V2 Codes. This is the file you want to edit.
  * The `*` refers to versioning, so instead of just `qr_config.cfg.bin` you might also see files such as `qr_config_0.04b.cfg.bin`. Pick the one with the highest version.

Once you've navigated to the file, open it in CfgBin Editor with the [latest mytags](../../modding-resources/cfgbin-tags.html).
In `QR2_INFO_LIST` (NOT `QR1_INFO_LIST` as that tree is for V1 QR Codes which has 36 times less slots):
* Click on the tree/top-level `QR2_INFO_LIST` and increase the number by one.
* Go to the last entry (i.e. `QR2_INFO_441` on the latest version of Psychic Specters), right click it and click duplicate.
* Increase `StartTypeDec` to the previous entries `EndTypeDec` + 1, and `EndTypeDec` to the new `StartTypeDec` + 1.
* Then change the `ItemID` to whatever you want :P
* Finally, lets generate some QRs for distribution: Get the `StartTypeDec` and convert it to base36 using a website like [this](https://www.unitconverters.net/numbers/decimal-to-base-36.htm).
* Then open [QRTool](https://n123git.github.io/QRTool/) - select V2 and then type the result under "Type" (Make sure the letters are capitalised!).
* Then select the amount of QRs to generate and enjoy!

## Yo-kai Watch B1/3/B2
First, navigate over to `data/res/qr`. You should see 1 type of file:
* `qr_config*.cfg.bin` - open it with CfgBin Editor.
  * The `*` refers to versioning, so instead of just `qr_config.cfg.bin` you might also see files such as `qr_config_0.04b.cfg.bin`. Pick the one with the highest version.
 
Once you've navigated to the file, open it in CfgBin Editor with the [latest mytags](../../modding-resources/cfgbin-tags.html).
In `QR2_INFO_LIST` (NOT `QR1_INFO_LIST` as that tree is for V1 QR Codes; which have 36 times less slots):
* Click on the tree/top-level `QR2_INFO_LIST` and increase the number by one.
* Go to the last entry (i.e. `QR2_INFO_2227` on one version of Yo-kai Watch 3), right click it and click duplicate.
* Increase `StartTypeDec` to the previous entries `EndTypeDec` + 1, and `EndTypeDec` to the new `StartTypeDec` + 1.
* Then decide if you want the QR Code to give the player 1 or 2 items:
  * If you want 1 item, set the `Item2ID` to `0`.
* Configure the rest of the params as you wish, `Flag1ID` and `Flag2ID` are `GlobalBitFlag`s that will be activated (set to `1`) once the QR Code has been scanned.
* Set `RandomQRTable` to 0
* Finally, lets generate some QRs for distribution: Get the `StartTypeDec` and convert it to base36 using a website like [this](https://www.unitconverters.net/numbers/decimal-to-base-36.htm).
* Then open [QRTool](https://n123git.github.io/QRTool/) - select V2 and then type the result under "Type" (Make sure the letters are capitalised!).
* Then select the amount of QRs to generate and enjoy!
