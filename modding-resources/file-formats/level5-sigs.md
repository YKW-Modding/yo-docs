---
title: Level-5 Signatures
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: false
---
# Platform Signatures

**Platform(s):** Nintendo 3DS, Android, Nintendo Switch, PSP, PS Vita, Windows, iOS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** 4 Byte Alignment<br/>
**String Encoding:** ASCII

In Level-5 file formats, you will often - but not always - encounter signatures.
Signatures (sometimes called magics) are sequences of bytes used to identify the type of the file - similar to what a file extension is supposed to do (but doesn't reliably in practice). Platform signatures (unofficial name), are a type of signature Level-5 commonly uses for their file formats, these appear at the start of the file (or file section since formats can be inlined in others e.g. `XPVB`) and follow the format `XXXYZZ`, where `XXX` is the main identifier, which actually identifies the file format (e.g., `IMG`, `ANM`, `CHR` etc), `Y` is the platform char (see the next section), and `ZZ` is the version number as a case-insensitive hex string (`00`, `01`, etc).

Here are some examples:
- `FNTC01`
- `ANMA00`
- `CHRC01`
- `IMGC00`

Here is the above shown with a good ol' ASCII diagram:
```js
IMGC00
││││└┴─ Version: 00
│││└─── Platform: C (Nintendo 3DS)
└┴┴──── Format: IMG
```

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

> [!NOTE]
> Name is not arbitrary; these are strings obtained from the binary of Yo-kai Watch 1 for Smartphone. The same applies to Code.
