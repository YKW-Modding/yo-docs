---
title: How to Create a Battle Rule
layout: default
grand_parent: Modding Guides
parent: Yo-kai and Battles
---
# How to Create a Battle Rule
> Original guide by @pqr29 on Discord; heavily modified by @n123original. This guide assumes you already know how to navigate romfs and use CfgBin Editor. If not, please read [the starting guide](../gettingstarted.html).

You will need Kuriimu2, CfgBin Editor and the latest MyTags, all of which you should have set up and be familiar with, by the end of the afformentioned [starting guide](../gettingstarted.html).

## 2. Create a BattleRule

* First, we will create a Battle Rule. To do this, extract and open `battle_rule_config*.cfg.bin` from `data/res/battle`.
  * The `*` refers to versioning, so instead of just `battle_rule_config.cfg.bin` you might also see ones such as `battle_rule_config_0.03g.cfg.bin`. Pick the one with the highest version.
* Then duplicate `BTL_RULE_INFO_2`, *then* `BTL_RULE_INFO_RULE_DATA_LIST_2`.
  * You will also want to increment the `ChildCount` (increase it by 1) of the `BTL_RULE_INFO_LIST` tree.
* Next open your new `BTL_RULE_INFO_*` entry (this should be the last one), and create a new `BtlRuleID`. No template is needed.
* Finally, modify the parameters to your heart's delight!
