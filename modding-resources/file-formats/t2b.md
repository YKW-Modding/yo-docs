---
title: t2b
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: true
---

# t2b

**Filetype:** Data<br/>
**File Extension(s):** `.cfg.bin`, `.astarbin`, `.chartbin`, `.npcbin`, `.pathbin`, `.actbin`, `.bin`<br/>
**Signature:** `\x01t2b`<br/>
**Platform(s):** Nintendo 3DS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** 4 Byte Alignment<br/>
**String Encoding(s):** cp932, UTF-8

*t2b*'s are structured binary configuration files used by 3DS Level-5 games for storing named entries containing typed values, configuring naerly all data within the games e.g. Items, Yo-kai, Quests, Maps etc.
It most likely stands for text 2 binary as it's the compiled version of a plaintext format which will be covered in a different page.
After the 3DS era of Level-5's engine, t2b files began to be superseeded by `RDBN` files of which we aren't aware of any similar plaintext format.

A t2b file contains the entry section, followed by the CRC section and then the footer, which always starts `0x10` bytes from the end of the file. Note that all offsets are relative to the *start* of their section. Meaning that entry string offsets are relative to the entry string table base and CRC string offsets are relative to the CRC string table base.

# Footer

| Field    | Type   | Description                                             |
| -------- | ------ | ------------------------------------------------------- |
| magic    | uint32 | Must be `0x62327401`.                                   |
| unk1     | Int16  | Seems to be constant?                                   |
| encoding | Int16  | String encoding, check the section below for specifics. |
| unk2     | Int16  | Seems to be constant?                                   |

### Encoding Values
If the high byte is `0x1` then the encoding is UTF-8, otherwise you must check the low byte. If the low byte is `0x1`, then the encoding is UTF-8, if `0x0` then CP932 (an extension of Shift-JIS). Note that both bytes must not contain a value aside from `0x1` or `0x0`. Meaning that the only valid encodings are as follows:
| Value    | Encoding          |
| -------- | ----------------- |
| `0x0000` | CP932 (Shift-JIS) |
| `0x0001` | UTF-8             |
| `0x0100` | UTF-8             |
| `0x0101` | UTF-8             |

TODO: rest
# Tree Structure

Although entries are stored as a flat list in the binary format, hierarchical structure is encoded using naming conventions. This child/parent (tree) structure is shown in the original plaintext source format and can be reconstructed by parsing entry names. A subtree is typically defined by a begin marker entry followed by child entries and terminated by a corresponding end marker.

## Hierarchy Markers

### Opening Markers
 
Entries that signal the start of a subtree typically end with one of the following *begin markers*:

```md
_BEGIN
_BEG
_BGN
_COUNT
```

These entries mark the *start* of a tree node, and all following entries up to the matching end marker are considered children of this node. Additionally, entries that terminate with a standalone underscore (`_`) may also function as opening markers in some cases.

> Note: Some opening marker entries may contain a `ChildCount` parameter. When present, this value is usually (but not always!) located as the last parameter in the entry's value list. Due to several inconsistencies it is not recommended to rely on this functionality and to instead leave it up to the user.

### Child Markers
Entries inside a subtree often carry info markers to indicate their type or role:

```md
_INFO
_DATA
_VAL
_ITEM
```

These are usually the actual payload or metadata for the tree node.

### Closing Marker
A subtree is terminated by a `_END` entry, which usually contains *no parameters*. The `_END` entry closes the most recent unclosed begin marker of the same prefix.

### Special PTREE Case
Level-5 frequently uses a separate convention for property tree roots: with `PTREE`, `PTVAL`, `PTVALS` and `_PTREE` - where `PTREE` is the `_BEG` equivalent, `PTVAL` and `PTVALS` the `_INFO` equivalent and `_PTREE` the `_END` equivalent.
This behaves like the normal `_BEGIN / _END` pattern but uses the custom naming scheme mentioned above.

> Note: Recursively apply the above steps as needed for nested subtrees.
