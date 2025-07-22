---
title: Adding QR Codes
layout: default
grand_parent: Modding Guides
parent: General Modding
---

# How to Add, Edit and Remove QR Codes
**Guide by @n123original on discord**

**CONFIRMED TO WORK ON YO-KAI WATCH 2.**

There are 3 QR code files. The 1st is for V1 QR codes (mainly used in YW1), V2 QR Codes, and Korean-Exclusive QR Codes. This guide will focus on V2 codes.

**Creating QRs**
  1. Get your ItemID of choice - if it's a negative number click Ctrl+Shift+I in your web browser of choice then click console and paste `[ItemID] >>> 0` where [ItemID] is your ItemID and you should get a `uint32` version.

**Editing Random QRs**

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
