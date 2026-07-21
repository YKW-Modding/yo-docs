---
title: Defining Triggers (YW2)
layout: default
parent: General Modding
grand_parent: Modding Guides
---

# Defining Triggers (YW2)
> Written by @n123original on Discord. This guide assumes you already know how to navigate romfs and use CfgBin Editor. If not, please read [the starting guide](../gettingstarted.html). Additionally, this guide does not explain how to write XQ, only how to define a trigger and attach code to it. Please consult the relevant guide(s) for learning how to program in XQ.

## Defining the Trigger

First, decide whether you want to create a global trigger or a map trigger:
* If you want a global trigger (available across all maps), open `data/res/sys/common_trigger*.cfg.bin`.
  * The `*` refers to versioning, so instead of just `common_trigger.cfg.bin`, you might also see files such as `common_trigger_0.03c.cfg.bin`. Pick the one with the highest version.
* If you want a map trigger (recommended for most NPCs), open `data/res/map/<MAP>/<MAP>.pck|<MAP>_trigger.cfg.bin`.
  * Meaning, go to `data/res/map`, open the folder associated with your map, open the `<MAP>.pck`, and then open `<MAP>_trigger.cfg.bin`.
    * For example, for Uptown Springdale it'd be `t101g00.pck|t101g00_trigger.cfg.bin`.

> [!WARNING]
> Due to generic key names, MyTags will not be useful here, **DO NOT attempt to write them**. The proposed fix has not been implemented to maintain backwards compatibility with old CfgBin Editors.

Once you've opened the file in CfgBin Editor:
* First, increment (increase by 1) the `ChildCount` on the `DATA_COUNT` entry.
* Next, duplicate a `DATA_ITEM` entry. The new entry should appear at the end of the tree.
* Next, configure the parameters of your new `DATA_ITEM`:
  * 1st param (`TriggerType`): The type of trigger you want to define. You should ideally know which type beforehand. Some common types include:
    * `11` (`0xB`) - NPCs
    * `21` (`0x15`) - FuncPoint Triggers
    * `39` (`0x27`) - SearchPoint Triggers
    * `71` (`0x47`) - AutoTrigger Triggers
  * 2nd param (`TriggerID`): The ID for your trigger. No template is needed for generating the hash.
  * 3rd param: Set to `0`.
  * 4th param (`Cond`): A CExpression. Leave as int `0` if not needed.
  * 5th param: Set to `0`.
  * 6th param: Set to `0`.
  * 7th param (`FunctionCallback`): Set to a value no other entry in the file has. The fastest and safest approach (which I therefore recommend), is to take the highest `FunctionCallback` value currently in the file and add 1 to it (e.g., if the highest is `99`, set yours to `100`).

## Attaching Code to the Trigger

Now that your trigger is defined, you now need to attach code that will execute when the trigger is activated, otherwise your trigger will be effectively useless.

* First, decompile the corresponding XQ script:
  * If you defined a global trigger, decompile `seq/common_trigger*.xq`.
  * If you defined a map trigger, decompile `data/res/map/<MAP>/<MAP>.pck|<MAP>.xq`.
    * The `*` refers to versioning, so instead of just `common_trigger.xq` you might also see files such as `common_trigger_0.03c.xq`. Pick the one with the highest version.
* Next, define a function called `RunCmd_Map<FunctionCallback>`. Take for instance, if you had a `FunctionCallback` of `100`:
```php
RunCmd_Map100()
{
  // insert code here, for example:
  $local1 = return_title();
}
```
* Finally, write the code you wish to be executed when your trigger has been activated, and recompile the XQ script.
