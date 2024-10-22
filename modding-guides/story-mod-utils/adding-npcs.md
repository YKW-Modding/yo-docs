---
title: Adding NPCs (YW2 & 3)
layout: default
parent: Create your own post game stories
grand_parent: Modding Guides
---

# Adding NPCs (YW2 & 3)

**Original author on Discord: @z.u.ra**

## Required tools
CfgBinEditor

Kuriimu2

NPCMake

Have XtractQuery installed **GLOBALLY**. You can find a text guide of this process [here](/modding-guides/xq-editing/g1.html) (at step 3). You can find a video version of the same guide [here](https://www.youtube.com/watch?v=30hn6VrURUs).

## The Guide

This guide is going to show how to use Npcmake's **GUI** version. I assume whoever wants to use the command line version is technical enough to know how to follow the guide using that version, as it's pretty trivial.

## Step 1: Generate An NPCMake Toml Template

Wait - What even is an NPCMake TOML?

**TOML** is a configuration file format where you can store various values. It looks like this:

```toml
MyVariable = "MyValue"
MySecondVariable  = 0
```
NPCMake uses TOML files with a specific subset of values to define NPC settings. How can you know what subset of values you need to add, you might ask? Well, this is where the **template** comes in. It generates a TOML with default NPCMake values so you have no trouble filling it out!

Here's how you can generate your very own template!

Open NPCMake and click the "Generate a template for npcmake TOML" button. This will open an explorer window where you can save your TOML file. Save this file anywhere you'd like! Just remember where it is, because we're gonna edit it in the next step-

## Step 2: Fill out your NPCMake TOML

**NOTE: This step explains the values on the TOML from NPCMake v1.1.0. I cannot guarantee this will work on future versions of NPCMake, but you should still try it, as I don't plan to change the TOML format much.**

Let's go over every value in the TOML template!

**NpcName** - This can be anything. For example, you could name your NPC "JibanyanNPCForMyMod" or you can name it "Bob". It literally doesn't matter.

**BaseId** - The BaseID your NPC should use. For custom Yo-kai, this can literally be anything, so check `chara_base`, but if you are using a vanilla Yo-kai, you can simply put your Yo-kai's model ID through a CRC32 hash.

**The next few properties are related to the NPC's position. For Yo-kai Watch 2, you unfortunately have to basically figure out what position you want for your NPC through trial and error. For YW3, however, you can get the position stored in the save file through [Yo-kai Editor 3](https://gbatemp.net/attachments/yo-kai-editor-3-rar.153374/) in the Info 2 section, making it much simpler to figure out a location for your NPC.**

**NpcX** - Your NPC's X position.

**NpcY** - Your NPC's Y position.

**NpcZ** - Your NPC's Z position.

**NpcRotation** - Your NPC's rotation in degrees.

**ChapterCode** - On which chapter would your NPC be talkable? (write c01 for chapter 1, c02 for chapter 2, etc. C11 is post game)

**MapID** - On which map ID are you adding your NPC to?

**OnTalk** - The XQ code to be executed when your interact with your NPC. You can learn how to code in XQ [here](/modding-guides/xq-editing.html).

**AppearCond** - A cond string that dictates when your NPC will be visible. Can be copied from other NPCs, be smart and edit a cond to your liking or leave as 0 for your NPC to be visible at all times.


## Step 3. Compile your NPC

Open NPCMake and click "Make from NPCMake TOML"

Click "Pick NPCMake TOML" and pick the TOML we created in step 2.

Click "Pick map folder with required files" and `data/res/map/[your map ID]` from the game's FA.

Click "Make NPC".

We are almost done! You should find the output files at `[Npc Name]_output` next to your TOML. The files inside are files from `data/res/map/[your map id]`, so simply replace the files there with the edited ones and you are golden!
