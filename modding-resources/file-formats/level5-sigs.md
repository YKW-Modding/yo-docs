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
**Signature:** N/A<br/>
**Platform(s):** Nintendo 3DS, Android, NX, PSP, PS Vita, Windows, iOS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** 2 Byte Alignment<br/>
**String Encoding:** ASCII

In Level5 file formats, you will often - but not always - encounter signatures.
Signatures (sometimes called magics) are sequences of bytes used to identify the type of the file - similar to what a file extension was supposed to do. Platform signatures, are a type of signature Level5 commonly uses, these appear at the start of the file/file section and follow the format `XXXYZZ`, where `XXX` is the main file-format unique signature (e.g., `IMG`, `ANM`, `CHR` etc), `Y` is the platform char (see the next section), and `ZZ` is the version number as a hex string (`00`, `01`, etc).

### Supported Platforms

| Char | Code | Name      | 
| ---- | ---- | --------- |
| W    | 0    | `win`     |
| C    | 1    | `ctr`     |
| P    | 2    | `psp`     |
| V    | 3    | `vita`    |
| A    | 4    | `android` |
| I    | 5    | `ios`     |
| N    | 6    | `nx`      |
