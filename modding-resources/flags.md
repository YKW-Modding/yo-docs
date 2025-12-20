---
title: Flag IDs
layout: default
has_children: true
parent: Modding Resources
---

# Flag IDs
Flag IDs are used all over the games to reference progression, events or any other situation where global data needs to be stored.

In Yo-kai Watch 1, 2, and B1:
* `FLAG_INFO_0` flags are boolean; (they can be a `1` or `0`); `FLAG_INFO_0` entries are internally referred to as `GlobalBitFlag`; these can be edited in XQ via `get_global_bitflag()` and it's set equivalent, and read in conds via `GetGlobalBitFlag`.
* `FLAG_INFO_1` flags are *8-bit unsigned integers* (meaning a number from 0-255); `FLAG_INFO_1` entries are internally referred to as `GlobalByteFlag`; a `FLAG_INFO_1` can be read in conds via `GetGlobalByteFlag`.
* `FLAG_INFO_2` flags are boolean; `FLAG_INFO_2` entries are internally referred to as `GlobalTBoxFlag`s; Cond's do have a CExpression Function called `GetGlobalTBoxFlag` although it seems to be no-opped (atleast in yw1).
* `FLAG_INFO_3` flags are similar to `FLAG_INFO_0` but are deemed "temporary". These can be read in conds via the CExpression function: `GetTempBitFlag`.
* `FLAG_INFO_4` flags are similar to `FLAG_INFO_3` but are *8-bit unsigned integers* (meaning a number from 0-255). These can be read in conds via the CExpression function: `GetTempByteFlag`.
In Yo-kai Watch 3:
* `FLAG_INFO_5` is unknown;
* `FLAG_INFO_6` flags are boolean bit flags (`0`/`1`) that reset every day, in CExpressions (Conds) these can be get/set using `GetOneDayBitFlag` and `SetOneDayBitFlag`.
* `FLAG_INFO_7` is unknown;
* `FLAG_INFO_8` is unknown;
* `FLAG_INFO_9` is unknown;
* `FLAG_INFO_10` is unknown;
> Note: Flags with the same ID can be in several `_INFO_*` list's. The most common example are `FlagID`s present in both `FLAG_INFO_0` and `FLAG_INFO_1`.

> Note: This resource will **NOT** cover map-specific flags; just flags accessible in the game-scoped cfg.bin present at `data/res/sys/flag_config_0.01.cfg.bin` which requires no execution context.
