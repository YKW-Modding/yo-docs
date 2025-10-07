---
title: YW2 -> YW3
layout: default
grand_parent: Map Modding
parent: Map Porting
has_children: false
nav_order: 2
---

# YW2 -> YW3
For Yo-kai Watch 2 to Yo-kai Watch 3 aside from BT maps (which wont be covered here) there are 2 main changes you need to make:
* Convert the mapenv - a W.I.P tool to assist with this can be found [here](https://github.com/n123git/yw-mapenvconv/).
* Convert the colbox `prm` to `XCL` for hitboxes.
  * As of this guide the only XCL reader does not have write capabilities - so this is currently not possible, a tool however is being developed for this.
