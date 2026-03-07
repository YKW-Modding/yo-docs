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
**Signature:** `t2b\x01`<br/>
**Platform(s):** Nintendo 3DS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** 4 Byte Alignment<br/>
**String Encoding(s):** cp932, UTF-8

The *t2b format* is a structured binary configuration format used by 3DS Level-5 games for storing named entries containing typed values, configuring naerly all data within the games e.g. Items, Yo-kai, Quests, Maps etc.
It most likely stands for text 2 binary as it's the compiled version of a plaintext format which will be covered in a different page.
After the 3DS era of Level-5's engine, t2b files began to be superseeded by `RDBN` files of which we only know the compiled format of.

# Format Layout

```
T2B File
├─ Entry Section
│  ├─ Entry Header
│  ├─ Entry Records
│  └─ Entry String Table
│
├─ CRC Section
│  ├─ CRC Header
│  ├─ CRC Records
│  └─ CRC String Table
│
├─ Footer (0x10 bytes from file end)
│
└─ Hi - idk I'm bored ok
```

Note that all offsets are relative to the *start* of their section. Meaning that entry string offsets are relative to the entry string table base and CRC string offsets are relative to the CRC string table base.

# Footer
The footer is *always* located at the absolute offset `fileSize - 0x10`.

## Structure

| Field    | Type   | Description                                         |
| -------- | ------ | ----------------==============================----- |
| magic    | uint32 | Must be `0x62327401`.                               |
| unk1     | Int16  | -                                                   |
| encoding | Int16  | String encoding. See the section below for details. |
| unk2     | Int16  | -                                                   |

### Encoding Values

| Value    | Encoding          |
| -------- | ----------------- |
| `0x0000` | CP932 (Shift-JIS) |
| `0x0001` | UTF-8             |
| `0x0100` | UTF-8             |
| `0x0101` | UTF-8             |

Note that all other encoding values are *Invalid* and should be treated as such.

# Entry Section

This section starts at absolute offset `0x00`.

## Entry Header

All the fields below are unsigned 32-bit integers:

| Field            | Description                       |
| ---------------- | --------------------------------- |
| entryCount       | Number of entries.                |
| stringDataOffset | Offset to the entry string table. |
| stringDataLength | Size of entry string table.       |
| stringDataCount  | Number of strings.                |

## Entry Record

| Field       | Type                     | Size                       | Description                                                                                                                   |
| ----------- | ------------------------ | -------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| crc32       | uint32                   | 4 bytes                    | CRC32 hash of the entry name.                                                                                                 |
| entryCount  | uint8                    | 1 byte                     | Number of values in this entry.                                                                                               |
| entryTypes  | packed `ValueType` array | variable                   | Types are packed sequentially. The size (in bytes) can be calculated via the following formula: `ceil(entryCount / 4)`.       |
| padding     | N/A                      | variable                   | Stream is forward-aligned to a 4-byte boundary after reading the type array. Padding consists of null bytes (`00`).           |
| entryValues | `Value[]`                | variable                   | Value array stored sequentially. The size (in bytes) can be calculated via the following formula: `entryCount * ValueLength`. |

# Entry Types

In one byte there are 4 types packed within it as it utilises 2 bits *per entry*. This means that it can be extracted via the following formula `(typeChunk >> (h * 2)) & 0x3`.

### ValueType

| Value | Meaning          |
| ----- | ---------------- |
| 0     | String           |
| 1     | Integer          |
| 2     | Float            |
| 3     | Invalid / unused |

After the type array the stream is aligned (to 4 bytes) with forward null padding. Note that here:
* String is an offset into the Entry String Table (negative = null string).
  * Strings in the Entry String Table are encoded with CP932/UTF-8 depending on the encoding byte in the footer.
* Integer is a signed 32-bit/64-bit integer.
* Float is an IEEE-754 single/double precision float.

# Entry Values
Whether values are 32-bit or 64-bit depends on the file. It is unknown where - or if - the format explicitly states value width. Parsers must detect whether values are 32-bit or 64-bit by attempting parsing and verifying section bounds.
This has no effect on strings but affects the Integer and Float types as mentioned above.

# Entry String Table

This table is located at `stringDataOffset` (which is defiend by the entry header). It is a continuous *non-compressed* blob of null-terminated strings. Encoding is defined in the footer of the t2b.
Entries reference strings using offsets into this table.

# CRC Section

This section is located *after* the entry section and its string table.

## CRC Header

| Field        | Type   | Description                          |
| ------------ | ------ | ------------------------------------ |
| size         | Uint32 | Size of the CRC section in bytes.    |
| count        | Uint32 | Number of CRC entries.               |
| stringOffset | Uint32 | Offset to the CRC string table.      |
| stringSize   | Uint32 | Size of the string table (in bytes). |

## Checksum Entry

The two fields defined below are both unsigned 32-bit integers:

| Field        | Description                                |
| ------------ | ------------------------------------------ |
| crc32        | CRC-32B (ISO-HLDC) hash of the entry name. |
| stringOffset | Offset to the name string.                 |

These entries map the `CRC-32` hash to the Entry Name.

# CRC String Table

This table is a continuous *non-compressed* blob of null-terminated strings representing the entry names. Encoding is defined in the footer of the t2b. Offsets are relative to the *first* string offset meaning they can be calculated as such:
`actualOffset = entry.stringOffset - firstEntry.stringOffset`. Note that CRC string offsets are relative to the CRC string table start, *NOT* the file start.

# Name Hashing

Entry names are hashed using CRC-32B (ISO-HLDC). **But** different `t2b`s have two different variants of it. They can either use the standard CRC-32 or CRC-32 JAM (Bitwise not of the standard CRC-32).
It isn't known if it is explicitly referenced in the file's header or someplace else therefore it is currently advised to detect which variant is being used by hashing the first string and comparing it with the stored CRC-32.

Finally, note that after parsing, the file should resolve to a structure similar to what's shown below:

```
T2B
 ├─ Entries[]
 │   ├─ Name
 │   └─ Values[]
 │       ├─ Type
 │       └─ Value
 ├─ Encoding
 ├─ ValueLength
 └─ HashType
```


