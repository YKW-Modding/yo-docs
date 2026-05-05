---
title: Level5 Signatures
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: false
---
# Platform Signatures

**Filetype:** N/A<br/>
**File Extensions:** N/A<br/>
**Signature:** `ARC0`<br/>
**Platform(s):** Nintendo 3DS, Android, NX, PSP, PSVita, Windows?, iOS/iPadOS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** 2 Byte Alignment<br/>
**String Encoding:** ASCII

In Level5 file formats, you will often - but not always - encounter signatures.
Signatures (sometimes called magics) are sequences of bytes used to identify the type of the file - similar to what a file extension was supposed to do. Platform signatures, are a type of signature Level5 commonly uses, these appear at the start of the file/file section and follow the format `XXXYZZ`, where `XXX` is the main file-format unique signature (e.g., `IMG`, `ANM`, `CHR` etc), `Y` is the platform code (see the next section), and `ZZ` is the version number as a hex string (`00`, `01`, etc).

### Supported Platforms
The platforms are checked in the following order:

| Char | Platform Code | Platform                       |
| ---- | ------------- | ------------------------------ |
| A    | 4             | Android                        |
| C    | 1             | Citrus (3DS)                   |
| I    | 5             | iOS/iPadOS? (not observed yet) |
| N    | 6             | NX (Nintendo Switch)           |
| P    | 2             | PSP                            |
| V    | 3             | PS Vita                        |
| W    | 6             | Windows? (same as NX)          |

Any other character will yield the Platform Code `0xFFFFFFFF` and is treated as invalid by their engine.

## Implementation
Platform signatures are read and validated using the following functions:
```cpp
uint64_t __thiscall LXP_CODE_BIN::GetPlatform(LXP_CODE_BIN *this);
int __thiscall LXP_CODE_BIN::GetVersion(LXP_CODE_BIN *this);
bool LXP_CODE_BIN::CheckFormat(const char* param) const;
```
