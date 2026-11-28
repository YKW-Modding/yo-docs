---
title: common/
layout: default
has_children: true
parent: data/
grand_parent: YO-KAI WATCH 4++ RomFS Legend
---

# common/

This folder contains:
* `action` - Contains files that dictate character AI in battles.
* `camera`
  * `config/external_camera_config.cfg.bin`
* `chara` - Contains `chara.cpk`, which unpacks the `chr` folder.
* `chr` - Contains every chr model in the game. A full list of IDs for this folder can be found at [this game’s ParamID page](https://light8227.github.io/yo-docs/modding-resources/param-ids/yw4-param-ids.html).
* `effect`
* `event`
* `event_cfg`
* `font` - Contains files that control how the game reads and displays the font.
* `gamedata/` - Contains most of the game's `cfg.bin` files for common configuration.
* `input`
  * `dbg_input_ctrl.cfg.bin`
  * `input_ctrl.cfg.bin`
  * `key_assign.cfg.bin`
* `map/` - Contains every map model in the game, as well as files for aspects that affect maps globally, such as weather.
* `menu` - Contains all of the models for UI or other related overlays.
* `movie` - Contains all of the prerendered movies in the game.
  * `L5logo.usm` - The LEVEL-5 logo shown when booting up the game.
  * `mov_drone_01.usm` - The animation of Hailey's Drone delivering packages.
  * `yw4_eved_0001.usm` - An unused version of the credits.
  * `yw4_eved_0002.usm` - The credits.
  * `yw4_evop_0001.usm`
  * `yw4_evop_0002.usm`
  * `yw4_evop_0011.usm`
  * `yw4_title_0001.usm`
  * `yw4_title_0002.usm`
* `property`
* `script/` - Contains all of the scripts in the game.
* `sound`
* `system` - Contains `add_content` and `app` configuration files.
* `text` - Contains all the text in the game separated by language.
  * `common` - Contains `_map.cfg.bin` files for `common_talk_text`, `help_list_text`, and `shop_serif_text`.
  * `de` - Seemingly a folder for German, but only has blank or placeholder files.
  * `en` - Seemingly a folder for English, but only has blank or placeholder files.
  * `es` - Seemingly a folder for Spanish, but only has blank or placeholder files.
  * `event` - Contains `_map.cfg.bin` files for every `ev` text file.
  * `fr` - Seemingly a folder for French, but only has blank or placeholder files.
  * `it` - Seemingly a folder for Italian, but only has blank or placeholder files.
  * `ja` - Contains all the text files in Japanese.
  * `map` - Contains `_map.cfg.bin` files for every `map` text file.
  * `ru` - Seemingly a folder for Russian, but only has blank or placeholder files.
  * `us_es`
  * `zh_hans` - Contains all the text files in Simplified Chinese.
  * `zh_hant` - Contains all the text files in Traditional Chinese.
