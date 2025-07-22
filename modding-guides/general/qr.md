---
title: Adding QR Codes
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# How to Add, Edit and Remove QR Codes
**Guide by @n123original on discord**

**CONFIRMED TO WORK ON YO-KAI WATCH 2.**

You will Need:
* `CfgBin Editor`.
  * Preferably the latest copy of `MyTags.txt`.
* The source code of `QRBel` - to test out/generate the QRs. This is available  [here](https://github.com/n123git/qrbel). Just click Code then Download Zip and extract the downloaded zip.
* A text or code editor - these come with any somewhat-modern OS i.e. Notepad for Windows.

There are 3 QR code files. The 1st is for V1 QR codes (mainly used in YW1), V2 QR Codes, and Korean-Exclusive QR Codes. This guide will focus on V2 codes.

**Creating QRs**
  1. Get your ItemID of choice.
  2. Open the 2nd `cfg.bin` in `yw2.a_fa/data/res/qr` - not `qr_config.cfg.bin` or `qr_config_ko_0.06c.cfg.bin` the `cfg.bin` is usually called `qr_config_0.04b.cfg.bin`.
  3. Click `QR2_INFO_LIST` - DO NOT open the dropdown yet.
  4. You should see a number - increase that number by 1.
  5. Create a new entry and look at the previous entry (used to be the last) pick the biggest number after it and add one - this will be an important number.
  6. Now in your new entry set the first 2 numbers to that number and the 3rd to your `ItemID`.
  7. Then get your number and run it through [here](https://www.unitconverters.net/numbers/base-10-to-base-36.htm), you should get a 1-3 digit combination of capital letters, numbers or both. This is your `type`. You will need this for the next section: *Testing/Generating QRs*.

**Testing/Generating QRs**

1. Download QRbel.
2. Open the `types.js` (with any text editor such as Notepad or a code editor if you have one on hand) in the `data` folder.
3. Get your `type`
4. You will see a structure like `"115": 2648996709,` - insert one like `"[type]": [randomNumber],` where `[type]` is your type and `[randomNumber]` is a positive, random number (remember this number).
5. Save your changes to `types.js` - then open `consumables.js` in the same place. In `consumable.js` scroll until you see something like this: <br/>
```js
{
  "_id": "562354621",
  "_name": "Loach"
},
```
6. You want to paste in another one of these but replace the long number with your `[randomNumber]` from earlier, and change the name to anything you want.
7. Save your changes and open `QRbel` (the `index.html`) with a web browser.
8. On the sidebar scroll until you see the name you selected and click on it.
9. You now have atleast a million new QR Codes - test one of them and distribute as many (or as little) as you want :D
10. Enjoy!

**Editing Random QRs**


Note: `0x9A89BF7C`.
