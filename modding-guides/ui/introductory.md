---
title: "§1: Introductory"
layout: default
parent: UIs
grand_parent: Modding Guides
nav_order: 1
---

# §1: Introductory
> [!WARNING]
> Please note that you are expected to already understand the basics of XQ itself. All code examples will utilise XtractQuery 6 syntax for readability.

In all 3DS Yo-kai Watch games, with the exception of Sangokushi (which will not be covered here), the content and functionality of UIs are not hardcoded; instead they are built from XQ scripts.
The following guides will teach you how to create, modify and otherwise interpret UIs. It is highly recommended to read each page in order. 
This guide is mainly targeted towards Yo-kai Watch 2, although it should, in general, apply to all aforementioned games.

A UI is simply an XQ script run with special context by the engine.
Objects created by a UI are bound to that UI in that they will no longer render once the UI's XQ is no longer running.
Therefore we maintain a main loop, a loop that lasts until the UI has been closed.
This will be explained in more detail further on.
The entry point for UIs (function called by the engine which starts all the logic) is named identically to its respective menu name.
For instance, `ScratchMenu` has the entry point `ScratchMenu()`. The XQ script responsible for an individual menu is located within `seq/menu/<MenuName>*.xq`

> [!NOTE]
> The `*` refers to versioning, so instead of only finding `ScratchMenu.xq`, you may, for example, also find a file named `ScratchMenu_009s.xq`. Select the file with the highest version.

The operations of a UI script can be split into 3 core stages:

  1. Loading - loading resources (this will be explained in detail in §3).
  2. Building - creating objects (this will be explained in detail in §2 and §4), assigning resources, spawning coroutines and other initialisation work.
  3. Listening - maintaining a main loop while handling user interactions (this will be explained in detail in §6).

For example, this technically functions as a UI, although being of little to no practical use:

```php
NotARealMenu()
{
  while(!ctrl_btn_held(5)) { // break main loop on Debug key (this can be pressed using an emulator)
    yield;
    if ctrl_btn_held(6) {
      Menu_MessageInfoDraw("You have pressed A!");
    }
  }
} /* all functions and instructions above will be covered during §5 */
```

We will use this as a base in §8 when we create a custom UI.
Finally, note that these stages are conventional, you, in theory, do not have to follow them, although it is recommended.

<!--
  TODO: overhaul the shitty 2022 XQ guides and link to those
  NOTE: account for root page on edit
-->
