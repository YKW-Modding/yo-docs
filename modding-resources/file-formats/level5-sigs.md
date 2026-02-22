---
title: Level5 Signatures
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: false
---
# Level5 Signatures

**Filetype:** N/A<br/>
**File Extensions:** N/A<br/>
**Signature:** `ARC0`<br/>
**Platform(s):** Nintendo 3DS, Android, NX, PSP, PSVita, Windows?, iOS/iPadOS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** 2 Byte Alignment<br/>
**String Encoding:** ASCII

In Level5 file formats, you will often - but not always - encounter signatures.
Signatures (sometimes called magics) are sequences of bytes used to identify the type of the file - similar to what a file extension is intended to accomplish. They are typically located at a fixed position within the file - usually the beginning, though there are exceptions.
Level5's signatures can generally be split into two types:

## Normal Signatures
* These are usually (but not always!) placed at the start of the file.
  * A common exception to this is the `t2b` format (often found with the file extension  `.cfg.bin`)
    * `t2b` file signatures are not padded and are located `0x0F` bytes from the *end* of the file as opposed to the beginning.
* They are sometimes padded to *two bytes* using null bytes (`00`).
* Normal signatures are typically composed of *ASCII-encoded* characters, e.g., `RDBN`.

## Platform Signatures
Platform signatures follow the format:
```
XXX
Y
ZZ
```

Where:
* `XXX` is the main signature (e.g., `IMG`, `ANM`, `CHR` etc).
* `Y` is the platform code.
  * Note that `W` and `N` share the same platform code - perhaps due to both of them having similarly-powerful hardware. 
* `ZZ` is the version number in hexadecimal (`00`, `01`, etc).

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

Any other character will yield the Platform Code `0xFFFFFFFF` and is treated as Invalid.

## Implementation
Platform signatures are read and validated using the following functions in `LXP_CODE_BIN`:
```cpp
uint64_t __thiscall LXP_CODE_BIN::GetPlatform(LXP_CODE_BIN *this);
int __thiscall LXP_CODE_BIN::GetVersion(LXP_CODE_BIN *this);
bool LXP_CODE_BIN::CheckFormat(const char* param) const;
```
