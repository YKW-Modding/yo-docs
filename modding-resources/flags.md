---
title: Flag IDs
layout: default
has_children: true
parent: Modding Resources
---

# Flag IDs
Flag IDs are used all over the games to reference progression, events or any other situation where a global data needs to be stored;

In Yo-kai Watch 2:
* `FLAG_INFO_0` flags are boolean; (they can be a `1` or `0`); `FLAG_INFO_0` is internally referred to as `GlobalBitFlag`; these can be edited in XQ via `get_global_bitflag()` and it's set equivalent, and read in conds via `GetGlobalBitFlag`.
* `FLAG_INFO_1` flags are *8-bit integers* (meaning a number from 0-255 or -128 to 127); `FLAG_INFO_1` is internally referred to as `GlobalByteFlag`; a `FLAG_INFO_1` can be read in conds via `GetGlobalByteFlag`.
* `FLAG_INFO_2` flags have an unknown purpose.
* `FLAG_INFO_3` flags have an unknown purpose.
* `FLAG_INFO_4` flags have an unknown purpose.

Note: Flags with the same ID can be in several `_INFO_*` list's. The most common example are `FlagID`s present in both `FLAG_INFO_0` and `FLAG_INFO_1`.
