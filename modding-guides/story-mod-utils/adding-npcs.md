---
title: Adding NPCs (NPCMake)
layout: default
parent: Creating Your Own Postgame Stories
grand_parent: Modding Guides
---

# Adding NPCs via NPCMake (YW1, 2, & 3)

**Original author on Discord: @z.u.ra, modified by @n123original**

## Required Tools

[NPCMake](https://github.com/SuperTavor/NPCMake)
> NPCMake is limited in what it can do, for alot of purposes I'd highly recommend manually creating them, although this guide will not cover that.

Have XtractQuery installed **GLOBALLY**. You can find a guide [here](/modding-guides/xq-editing/g1.html) (at step 3). You can find a video version of the same guide [here](https://www.youtube.com/watch?v=30hn6VrURUs). I generally do not recommend video guides, but it was already here, as I was modifying this guide, so I'm not going to remove it :p

## Explanation
This guide is going to show how to use Npcmake's **GUI** version. I'd assume anyone intending to use the CLI is 'technical' enough to be able to transfer the GUI instructions to the CLI, as it's pretty trivial.

## Step 1: Generate An NPCMake Toml Template
Wait - What even is an NPCMake TOML?

*TOML* is a configuration file format where you can store various values. It looks like this:

```toml
MyVariable = "MyValue"
MySecondVariable  = 0
```
NPCMake uses TOML files with a specific subset of values to define basic NPC settings. How can you know what subset of values you need to add, you might ask? Well, this is where the **template** comes in. It generates a TOML with default NPCMake values so you have no trouble filling it out!
Here's how you can generate your very own template:
 * Open NPCMake and click the "Generate a template for npcmake TOML" button.
   * This will open an explorer window where you can save your TOML file. Save this file anywhere you'd like! Just remember where it is, because we're gonna edit it in the next step
     * This brings us onto the next step!

## Step 2: Fill Out your NPCMake TOML
> **NOTE: This step explains the values on the TOML from NPCMake v1.2.0. I cannot guarantee this will work on future versions of NPCMake, but you should still try it, as I don't plan to change the TOML format much.**

Let's go over every value in the TOML template!
* `NpcName`
  * This can be anything. For example, you could name your NPC "JibanyanNPCForMyMod" or you can name it "Bob". It literally doesn't matter.
* `BaseId`
  *  The `BaseID` of your NPC should use. For custom Yo-kai, this can literally be anything, so check `chara_base`, but if you are using a vanilla Yo-kai, you can simply put your Yo-kai's model file name through a CRC32 hash.

The next few properties are related to the NPC's position.
For non-YW3 games, you could:
* Use n123's save editor (Recommended for beginners).
* Use [CTRPF](https://github.com/PabloMK7/CTRPluginFramework-BlankTemplate).
* Use a basic XQ edit (Recommended for advanced modders).
* And more ways, none of these will be explained in detail here.

For YW3, you can:
* Use n123's save editor (Recommended).
* Use [Togenyan's save editor](https://github.com/nobodyF34R/ykw-editors).
* Use [CTRPF](https://github.com/PabloMK7/CTRPluginFramework-BlankTemplate).
* Use a basic XQ edit (Recommended for advanced modders).
<br>

* `NpcX`
  * Your NPC's X position.
* `NpcY`
  * Your NPC's Y position.
* `NpcZ`
  * Your NPC's Z position.
* `NpcRotation`
  * Your NPC's rotation in degrees.
* `ChapterCode`
  * From which chapter, will the player be able to interact (talk) with your NPC.
    * Write c01 for chapter 1, c02 for chapter 2, and so on - c11 is post game.
* `MapID`
  * Determines which Map the NPC will be placed in.
* `OnTalk`
  * The XQ code to be executed when your interact with your NPC. You can learn how to program in XQ [here](/modding-guides/xq-editing.html).
* `AppearCond`
  * A CExpression string that determines when your NPC will be visible. Can be copied from other NPCs, modified or compiled using yw-cond.
* `IsYw1`
  * A boolean variable that specifies if you are adding your NPC to Yo-kai Watch 1.
    * In short, leave as false if you are working with YW2, YW3 or YWB - otherwise, change it to true.
* `NpcType`
  * Dictates what icons NPCs have on the bottom screen map. The built in NPC types are the following:
    * Human
    * Yokai
  * Simply type one of these as a string in the value of NpcType for it to work. Alternatively, if you want to use a NpcType that is not included, replace the value with an integer value representing your NPCType.

## Step 3. Compile your NPC
* Open NPCMake and click "Make from NPCMake TOML"
* Click "Pick NPCMake TOML" and select the TOML we created in step 2.
* Click "Pick map folder with required files" and select a folder containing your extracted FA.
* Click "Make NPC".

We are almost done! You should find the output files at `[Npc Name]_output` next to your TOML. The files inside are a skeleton of the FA structure, but only containing the edited files! To include these in your mod, simply paste all the folders in `[Npc Name]_output` into your mod folder and it should merge the folders correctly.
