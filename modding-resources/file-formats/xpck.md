---
title: XPCK
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: true
---

# XPCK

**Filetype:** Archive<br/>
**File Extension(s):** `.pck`, `.xc`, `.xv`, `.xa`, `.xb`, `.xk`, `.xf`, `.xr`<br/>
**Signature:** `XPCK`<br/>
**Platform(s):** Nintendo 3DS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** 4 Byte Alignment<br/>
**String Encoding:** ASCII

The XPCK file format is a generic flat archive (no directories) used to bundle related files together. The purpose of the XPCK in question can generally be inferred based on the file extension as shown below:
| Extension   | Description                                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------------------------- |
| `.pck`      | A somewhat rare file extension that is completely generic in usage.                                                       |
| `.xc`       | Used for model files containing many, many different properietary file formats.                                           |
| `.xv`       | Holds camera files, including `CMR.bin` and `.cmr2`.                                                                      |
| `.xa`       | Named **XAnm**, contains `ANM` resources: 2D textures, UVs, and other data for building UIs along with XQ scripts.        |
| `.xk`       | Named **XSky**, used to configure fog, sky, and related data. Includes `SKY.bin`, `FOG.bin`, `SUB.bin`, and `.lgt` files. |
| `.xf`       | Known as **XFont** files, used to define custom fonts.                                                                    |
| `.xr`       | Generic resource bundle. Can contain 2D or 3D assets, though mostly 2D.                                                   |
| `.xb`       | -                                                                                                                         |


## Format Layout
```cpp
XPCK Header

File Entry Table
Compressed Name Table
File Data Section (4-byte aligned)
```

Note that all offset and size fields are stored in units of 4 bytes (32-bit words). Meaning to derive the offset in bytes you must shift them by 2 (`<< 2`).
## Header

| Offset | Size | Type    | Name             | Notes                                                                                                                   |
| ------ | ---- | ------- | ---------------- | ----------------------------------------------------------------------------------------------------------------------- |
| 0x00   | 4    | char[4] | magic (`XPCK`)   | Magic. Must always be equal to `0x4B435058` (ASCII representation of `XPCK`).                                           |
| 0x04   | 2    | u16     | fileCountAndType | Read the notes section under the table for this.                                                                        |
| 0x06   | 2    | u16     | infoOffset       | `infoOffset << 2` defines the start of the File Entry table.                                                            |
| 0x08   | 2    | u16     | nameTableOffset  | `nameTableOffset << 2` defines the start of the *compressed* Name Table.                                                |
| 0x0A   | 2    | u16     | dataOffset       | Offset into the file data section.                                                                                      |
| 0x0C   | 2    | u16     | infoSize         | Size of the File Entry Table section in 32-bit words; `infoSize << 2` is equivalent to the size in bytes.               |
| 0x0E   | 2    | u16     | nameTableSize    | Size of the *compressed* Name Table section in 32-bit words; `nameTableSize << 2` is equivalent to the size in bytes.   |
| 0x10   | 4    | u32     | dataSize         | Size of the file data section in 32-bit words; `dataSize << 2` is equivalent to the size in bytes.                      |

Notes:
* The u16 `fileCountAndType` contains the `fileCount` and `contentType` which can be derived using the following formula:
```cpp
fileCount = fileCountAndType & 0xFFF;
contentType = fileCountAndType >> 12;
```

* The following fields are stored divided by 4 (meaning you must `<< 2` to convert to actual offsets):
* `infoOffset`
* `nameTableOffset`
* `dataOffset`
* `infoSize`
* `nameTableSize`
* `dataSize`

## File Entry Table
The file entry table is located at the offset given by `infoOffset << 2`, with there being `fileCount` entries - each of size `0xC` - with the structure being shown below. Therefore the size (in bytes) can be calculated as: `fileCount * 0xC`.

### Structure
| Offset | Size | Type | Name            | Notes                                                                                                              |
| ------ | ---- | ---- | --------------- | ------------------------------------------------------------------------------------------------------------------ |
| 0x00   | 4    | u32  | hash            | CRC32-B of the filename string                                                                                     |
| 0x04   | 2    | u16  | nameOffset      | Offset relative to the start of the *decompressed* name table. Decompress the table before utilising this pointer. |
| 0x06   | 2    | u16  | fileOffsetLower | -                                                                                                                  |
| 0x08   | 2    | u16  | fileSizeLower   | -                                                                                                                  |
| 0x0A   | 1    | u8   | fileOffsetUpper | -                                                                                                                  |
| 0x0B   | 1    | u8   | fileSizeUpper   | -                                                                                                                  |

Note that the `fileOffset` is relative to `dataOffset` and can be calculated using the following formula:
```cpp
fileOffset = ((fileOffsetUpper << 16) | fileOffsetLower) << 2;
```
* `fileSize` calculation is similar but without the bit shift:
```cpp
fileSize = (fileSizeUpper << 16) | fileSizeLower;
```

## Name Table
The Name Table is a compressed section located at `nameTableOffset << 2`, of (compressed) size `nameTableSize << 2` - it is composed of sequential null-terminated ASCII filenames. Filenames are expected to be unique within an archive.

### Structure
No table is needed for this as it's just - as stated earlier - a continuous sequence of *unique* null-terminated ASCII filenames. 

## Entry Ordering
Note that when writing, files must be sorted alphabetically by name when assigning offsets and the entry table must be sorted by `hash` (CRC-32B) ascending.
