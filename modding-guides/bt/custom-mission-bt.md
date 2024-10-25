---
title: Adding New Missions to Blasters T
layout: default
grand_parent: Modding Guides
parent: Blasters T
---

# Adding New Missions to Blasters T
**Original guide by @thewadi on discord**

**Step 1 - Adding a New entry to mission_5.00.33.cfg.bin**

Make sure to use Cfg.Bin Editor with the latest tags

Open `res/hackslash/missions/mission_5.00.33.cfg.bin`

Duplicate the `MISSION_CONFIG_INFO_LIST_80`

Create a `MissionID`

Set a `MissionNumber` superior than 997500 (the mission number is the name of the folder where the enemy and bosses are configured in res/missions)

Set a `MissionType` (1 is a Standard mission, 2 is a Dangerous Mission, 3 is a Forbidden Mission and 4 is a Aura Mission)

Set the `NumberOfFloor`with the number of floor of your dungeon

Set your `MissionMenuNumber` to a number superior than 97

Set your `MissionPhaseAppear` to 0 (since we didn't discovered how to make custom condition at the time of I'm writing this guide)

Set a MissionTextID (The mission's text cfg.bin is in the .fa language in mission_common_text_en.cfg.bin) (and of course I'm not going to tell you here how to add a text entry to a cfg.bin)

You can also set an item to be used to access the mission by putting its ID in `ItemUnlockID`

**Step 2 - Adding Rewards to your Mission**

Open `res/hackslash/mission/mission_reward_nuparts_0.01r.cfg.bin`

Duplicate two entries with the same number (normally there is one entry and one entry group with the same name)

Set your MissionID in REWARD_INFO_*YourEntryNumber*

Set 3 rewards (it can be either an item or a yo-kai, if it's a yokai, put "1" to `IsYokai` and "0" to `IsItem`)

**Step 3 - Adding the Menu Information to your Mission**

Duplicate an entry

Set your `MissionID`

Set the BaseID of your Enemies Yo-kai in `EnemyBaseID`s (Minimum 4)

Set the BaseID of your Boss in `BossBaseID`

**Step 4 - Adding the Enemies and the Boss to your Mission**

Duplicate a folder in `res/hackslash/missions` and rename it with your `MissionNumber` You've set in the Step 1 ***Exemple: "mi010400"***

Do the same with the ..._enemy_0.01t.cfg.bin inside (rename it with your `MissionNumber`)

Set your `EnemyParamIDs` and your `BossParamID`

**Step 5 - Making Your Boss' AI Work**

Open res/character/hackslash

Find the ParamID of an existing Mini Boss (example: Bastenyan)

Duplicate the HACKSLASH_BOSS_CHARA_PARAM_INFO_LIST entry of the Mini Boss and set your Mission's Boss ParamID

**Step 6 - Create a Second ParamID for Your Boss with More HP and Stats**

The title says it all, Make sure to put the stats in `Min` and `Max` in charaparam
