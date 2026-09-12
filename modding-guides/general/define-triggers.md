---
title: Defining Triggers
layout: default
parent: General Modding
grand_parent: Modding Guides
---

# Defining Triggers
> Written by @n123original on Discord. This guide assumes you already know how to navigate romfs and use CfgBin Editor. If not, please read [the starting guide](../gettingstarted.html). Additionally, this guide does not explain how to write XQ, only how to define a trigger and attach code to it. Please consult the relevant guide(s) for learning how to program in XQ.

This guide will explain both how to define a trigger, and attach XQ code to it. The first section will explain what a trigger is, where they are defined, and their parameters. It is not recommended to skip it.

## What is a Trigger
The trigger system is used to run code on certain interactions or events. For instance, talking to an NPC, engaging with a crosswalk, or sleeping.
The event is determined entirely by the calling mechanism, that is, the code which runs the trigger. Each Trigger has a `TriggerType` which determines the calling mechanism, for instance, `GoodBoyTriggers` are ran on the player exercising good conduct on a crosswalk. There are 3 main categories:
* Common Trigger. This is the generic form, most globally available Triggers fall under the Common Trigger category.
* Map Trigger. Map Triggers are on a per-map basis, e.g. you may define a Map Trigger for Uptown Springdale. These are higher priority than Common Triggers, thus they will override them.
* Phase Trigger. Phase Triggers are on a per-chapter basis, e.g. you may define a Phase Trigger for Chapter 2. These are the highest priority, thus they will override both Common and Map Triggers.

Each category has both a config file and an XQ script per-basis. For instance, each individual map may have its own config file for its respective Map Triggers. The file paths for each config file, can be found below: <!-- --> <br id="trig-cfg-file-path"></div>

* Common Trigger: `data/res/sys/common_trigger*.cfg.bin`.
  * The `*` refers to versioning, so instead of only finding `common_trigger.cfg.bin`, you may also find a file such as `common_trigger_0.03c.cfg.bin`. Select the highest version file.
* Map Trigger: `data/res/map/<MAP>/<MAP>_trigger.cfg.bin` (YW1), `data/res/map/<MAP>/<MAP>.pck|<MAP>_trigger.cfg.bin` (YW2+).
  * `<MAP>` is a placeholder, and `|` selects the file within the archive. For example for YW2, go to `data/res/map`, open the folder associated with your map, and open the `<MAP>.pck`, e.g. for Uptown Springdale in most games it'd be `t101g00.pck`. Then inside the `.pck`, find the trigger file associated with your map.
* Phase Trigger: `data/res/phs/<CHP>/<CHP>_trigger*.cfg.bin`.
  * `<CHP>` is a placeholder, substitute it with your chapter of choice, for instance `c02` for Chapter 2, `c03` for Chapter 3 etc.
  * The `*` refers to versioning, so for example, instead of only finding `c05_trigger.cfg.bin`, you may also find a file such as `c05_trigger_0.03c.cfg.bin`. Select the highest version file.

Next, I have provided below a similar list instead referring to the location of the associated XQ script. <br id="trig-xq-file-path"></div>

* Common Trigger: `seq/sys/common_trigger*.xq`
  * The `*` refers to versioning, so instead of only finding `common_trigger.xq`, you may also find a file such as `common_trigger_0.03c.xq`. Select the highest version file.
  * Another similar script can also be found at `seq/common_trigger*.xq` from Yo-kai Watch 2 onwards. The distinction is irrelevant for this guide.
* Map Trigger: `data/res/map/<MAP>/<MAP>.xq` (YW1), `data/res/<MAP>/<MAP>.pck|<MAP>.xq` (YW2+).

> [!NOTE]
> Please note that this is not to be confused with the directory `seq/map`. The purpose of said directory will not be covered in this guide, as it is not relevant.
> Nor is it to be confused with the hardcoded `seq/t100t00.xq`. This file is utilised for trains, and is similarly irrelevant. 

* Phase Trigger: `seq/phs/<CHP>_trigger*.xq`.

Next, I will explain the contents of said script files. It is assumed that you are already aware of the basics of XQ Scripting. If not, please refer to the related guide.
Within these scripts, one will find functions. These functions are called by their related triggers. The triggers that execute a function can be found by matching the function name to the trigger's `FunctionCallback` and `RpdFunctionCallback`. These are properties of a Trigger that will be explained in more detail later. For instance, a common trigger with `FunctionCallback` 5 and `RpdFunctionCallback` 7 calls both `RunCmdRpd_Common7` and `RunCmd_Common5` on execution.

Map and Phase Triggers work similarly, except the `Common` term preceding the number in the function name is replaced with `Map` and `Phase` respectively. 
Finally, I will explain a Trigger's properties. These are contained within the associated config file.

Within a config file you will first see a `DATA_COUNT` entry. This entry has one property, the `ChildCount`; this is the amount of `DATA_ITEM` entries following it. Thus, when creating a trigger we increase the `ChildCount` by one. Next, every proceeding `DATA_ITEM` entry is a Trigger. 

> [!WARNING]
> Due to generic key names, MyTags will not be useful here, **DO NOT attempt to write them**. The proposed fix has not been implemented to maintain backwards compatibility with old CfgBin Editors.

Here are the parameters within a `DATA_ITEM` entry; the trigger's properties:<br id="trig-cfg-file-properties"></div>

  * 1st param (`TriggerType`): The type of trigger you want to define, this determines the calling mechanism as explained earlier. You should ideally know which type beforehand. Some common types include:
    * `11` (`0xB`) - NPCTrigger
    * `21` (`0x15`) - FuncPointTrigger
    * `39` (`0x27`) - SearchPointTrigger
    * `40` (`0x28`) - EnvTimeTrigger
    * `46` (`0x2E`) - GoodBoyTrigger
    * `47` (`0x2F`) - BadBoyTrigger
    * `71` (`0x47`) - AutoTrigger
    * `85` (`0x55`) - WatchmapStartTrigger
    * `87` (`0x57`) - WatchmapLensTrigger
  * 2nd param (`TriggerID`): The Main ID for your Trigger. TriggerIDs are used to determine which Trigger of the expected TriggerType to execute. However, not all TriggerTypes have unique variants sorted by IDs. For instance, two TriggerType 11 Triggers may be called by completely different situations, thus they require TriggerIDs. However, for instance, `GoodBoyTriggers` (and its negative counterpart `BadBoyTriggers`) are all executed on an event triggered by the player's conduct on the crosswalks. Hence, all Triggers of these types have TriggerID 0, simply being narrowed down based on the Condition. No single template is needed for generating the TriggerID for all trigger types, although some are expected to have one. For instance:
    * EnvTimeTriggers use template `<HOUR>`, which determines when it is executed.
    * GoodBoyTriggers and BadBoyTriggers use ID `0`.
    * WatchmapStartTriggers and WatchmapLensTriggers use template `<MAP>`.
  * 3rd param (`TriggerID2`): `TriggerID2` is only meaningful for a minority of TriggerTypes. It is a secondary ID used to filter which trigger is ran, similarly to `TriggerID`. The exact meaning is on a per-TriggerType basis.
  * 4th param (`Condition`): The CExpression that determines whether the Trigger can be executed.
  * 5th param (`RpdFunctionCallback`): As mentioned earlier, `RpdFunctionCallback` acts similarly to `FunctionCallback`, however it instead calls a `Rpd` function. It is valid to have a Trigger with both fields set to a non-zero value, in fact, nearly all triggers which have a non-zero `RpdFunctionCallback`, have the same value for their `FunctionCallback`. 
  * 6th param (`Unk-AlwaysZero`): This is a reserved (unused) field in practice, but it seems to have functionality linked to it, I have not traced what this functionality is yet. 
  * 7th param (`FunctionCallback`): As mentioned earlier, this specifies the function called, it is typical to set this to `0` if not needed.

## Defining the Trigger

Decide which scope and category your trigger belongs to and open the corresponding config file. Please refer to [the paths listed above](#trig-cfg-file-path) for your chosen category. For most NPC-related triggers, a Map Trigger is recommended; for functionality that should be globally available, use a Common Trigger.

> [!WARNING]
> Due to generic key names, MyTags will not be useful here, **DO NOT attempt to write them**. The proposed fix has not been implemented to maintain backwards compatibility with old CfgBin Editors.

Once you've opened the file in CfgBin Editor:

* First, increment (increase by 1) the `ChildCount` on the `DATA_COUNT` entry.
* Next, duplicate a `DATA_ITEM` entry. The new entry should appear at the end of the tree.
* Next, configure the parameters of your new `DATA_ITEM` entry (see [the explanation above](#trig-cfg-file-properties) for the purpose of each field):
  * `TriggerType`. You should ideally know which type beforehand, see the above list.
  * `TriggerID`. Follow the template for your chosen type, if one applies.
  * `TriggerID2`. Set to `0` unless your TriggerType expects otherwise.
  * `Condition`. Leave as int `0` if not needed, otherwise insert your CExpression in base64 form.
  * `RpdFunctionCallback`. Set to `0` unless you need to execute a `Rpd` function.
  * `Unk-AlwaysZero`. Set to `0`.
  * `FunctionCallback`. Set to a value no other entry in the file has. The fastest and safest approach (which I therefore recommend), is to take the highest `FunctionCallback` value currently in the file and add 1 to it (e.g., if the highest is `99`, set yours to `100`).

## Attaching Code to the Trigger

Now that your trigger is defined, you now need to attach code that will execute when the trigger is executed, otherwise your trigger will be effectively useless.

* First, decompile the corresponding XQ script for your trigger (refer to [the explanation above](#trig-xq-file-path) for the location).
* Next, define the appropriate function for your Trigger. Take for instance, if you had a `FunctionCallback` of `100` for a Map Trigger, insert:
```php
RunCmd_Map100()
{
  // insert code here, for example:
  $local1 = return_title();
}
```

* Finally, within the function, insert the code you wish to be executed when your trigger has been activated, and recompile the XQ script.
