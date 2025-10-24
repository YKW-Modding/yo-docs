---
title: yw2_a.fa
layout: default
has_children: true
parent: Yo-kai Watch 2 RomFS Legend!
grand_parent: RomFS Legend
---
# yw2_a.fa
This is the main game archive containing all assets and data that dosent change across localisations. 
> **Note: Some Exceptions such as Wanted Criminal `cfg.bin`s and KOR QR Codes are still stored in this archive for developement reasons.**
It contains:
* `ctr/` - Game Icons.
* `sky/` - A folder containing `default.xk`, an archive of skybox data.
* `fnt/` - A folder containing `ft_nrm.xf` and `ft_sml.xf` - the games font files. Can be opened with Kuriimu2 or XFontEditor
* `seq/` - A folder containing the game's `.xq` scripts which set up events, UI etc
* `data/` - A folder containing everything else.
* `fix.xr` - an XPCK encoded archive which contains important data i.e. home menu icons, shader data etc
