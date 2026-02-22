---
title: ARC0 (.fa)
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: true
---

# ARC0

**Filetype:** Archive<br/>
**File Extensions:** `.fa`<br/>
**Signature:** `ARC0`<br/>
**Platform(s):** Nintendo 3DS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** 4 Byte Alignment<br/>
**String Encoding:** SJIS*
> *The default (japanese) encoding used by the 3DS games' is actually cp932 (an extension of sjis) although the difference is negligible - this was likely just an unintended result of the libraries used by Level5.

ARC0 Archives are the main game archives which hold all of romfs (with the exception of audio) within Level5's 3DS games. In Yo-kai Watch there are two forms of ARC0 archives:
* Main game archive
  * Structure: `<game>_a.fa`
  * This holds most of the game data - in Yo-kai Watch games this archive is called either `yw2_a.fa` (YW2), `yw1_a.fa` (YW1), or `yw_a.fa` (YWB+) depending on the game.
* Locale archive
  * Structure: `<game>_lg_<lg>.fa`
  * There are several of these - one for each locale the copy of the game supports, with the player's locale deciding which one is used. These have priority over the main archive so the game checks to see if the file is in the locale archive first. This is commonly used for text and assets which contain text as they are language-dependant. Some example locale archives can be found below:
    * `yw2_lg_engb.fa`
    * `yw1_lg_fr.fa`
    * `yw_lg_es.fa`

The two forms have the same binary layout shown below:
```
Header (0x48/72 bytes)

Compressed Directory Entry Table
Compressed Directory Hash Table
Compressed File Entry Table
Compressed Name Table

File Data Section (uncompressed, 4-byte aligned) <- this is likely because basically all of level5s file formats are already compressed; compressing a compressed file won't save much due to the increased entropy
```

Note that all tables except file data are stored inside Level-5's Compressed Container structure and all major sections are padded to a 4-byte alignment.

## ARC0 Header
### Structure

| Offset | Size | Type    | Name                   |
| ------ | ---- | ------- | ---------------------- |
| 0x00   | 4    | char[4] | magic (`ARC0`)         |
| 0x04   | 4    | u32     | directoryEntriesOffset |
| 0x08   | 4    | u32     | directoryHashOffset    |
| 0x0C   | 4    | u32     | fileEntriesOffset      |
| 0x10   | 4    | u32     | nameOffset             |
| 0x14   | 4    | u32     | dataOffset             |
| 0x18   | 2    | u16     | directoryEntriesCount  |
| 0x1A   | 2    | u16     | directoryHashCount     |
| 0x1C   | 4    | u32     | fileEntriesCount       |
| 0x20   | 4    | u32     | tableChunkSize         |
| 0x24   | 4    | u32     | reserved1 (always 0)   |
| 0x28   | 4    | u32     | unk2                   |
| 0x2C   | 4    | u32     | unk3                   |
| 0x30   | 4    | u32     | unk4                   |
| 0x34   | 4    | u32     | unk5                   |
| 0x38   | 4    | u32     | directoryCount         |
| 0x3C   | 4    | u32     | fileCount              |
| 0x40   | 4    | u32     | unk7                   |
| 0x44   | 4    | u32     | reserved2 (always 0)   |

Leave the unk entries as-is when parsing ARC0 archives. `unk2`-`unk5` may be hashes. Also note that:
* `fileCount` is the number of files in this directory  
`tableChunkSize` is the total size of all tables before the file data section, aligned to 4 bytes. It is calculated as follows:
```cpp
tableChunkSize = (directoryEntriesCount * 20) + (directoryHashCount * 4) + (fileEntriesCount * 16) + nameTableDecompressedSize + 0x20
```

## Directory Entry Table
This section is compressed with Level5's Compressed Container format - the decompressed format is shown below:

### Structure

| Offset | Size | Type | Name                     |
| ------ | ---- | ---- | ------------------------ |
| 0x00   | 4    | u32  | crc32                    |
| 0x04   | 2    | u16  | firstDirectoryIndex      |
| 0x06   | 2    | s16  | directoryCount           |
| 0x08   | 2    | u16  | firstFileIndex           |
| 0x0A   | 2    | s16  | fileCount                |
| 0x0C   | 4    | s32  | fileNameStartOffset      |
| 0x10   | 4    | s32  | directoryNameStartOffset |

Note that:
* `firstFileIndex` is an index into file entry table.
* `firstDirectoryIndex` references the index in the sorted directory table.
* `directoryCount` = number of direct subdirectories.
* `fileCount` = number of files in this directory.
* `directoryNameStartOffset` = offset inside name table.
* `fileNameStartOffset` = offset inside name table.
* For the root directory `crc32` = `0xFFFFFFFF`.

## Directory Hash Table
This section is compressed with Level5's Compressed Container format - the decompressed format is shown below:

* 4 bytes per entry
* Contains CRCs of directories where:

  ```
  crc32 != 0xFFFFFFFF
  ```
* Order matches sorted directory entry order

## File Entry Table
This section is compressed with Level5's Compressed Container format - the decompressed format is shown below:

### Structure

| Offset | Size | Type | Name               |
| ------ | ---- | ---- | ------------------ |
| 0x00   | 4    | u32  | crc32              |
| 0x04   | 4    | u32  | nameOffsetInFolder |
| 0x08   | 4    | u32  | fileOffset         |
| 0x0C   | 4    | u32  | fileSize           |

Note that:
* Entries are grouped by directory and within a directory, entries are sorted in the order: CRC-32 ascending
* `crc32` is computed from filename only
* `nameOffsetInFolder` is relative to the directory's `fileNameStartOffset`
* `fileOffset` is relative to `dataOffset`
* `fileSize` is uncompressed size
* File data is 4-byte aligned

## Name Table
This section is compressed with Level5's Compressed Container format - the decompressed format is shown below:

### Structure
This section entirely consists of a continuous blob of mull-terminated directory names and null-terminated file names. Directory names must end with a forward slash followed by a null terminator (`"/\x00"`) - with the exception of the root directory which is an empty string (`"\x00"`). The trailing `/` is included in the `CRC-32` calculation. The `CRC-32` of files only contains the file name and not the directory as stated in the File Entry table.

## File Data Section
This section begins at `dataOffset` and requires that files are written sequentially, with each file being aligned to a 32-bit word (4 bytes). Note that *NO* compression is applied at the `ARC0` level - it is left up to the files contained within it to compress themselves as part of their individual file formats.
