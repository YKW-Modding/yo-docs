---
title: Conds (Conditions)
layout: default
has_children: false
parent: Modding Resources
---

Conds are structured, binary structure representing a **boolean condition** in Yo-kai Watch games. They are stored as base64 strings in `cfg.bin`s and control conditions for everything from NPCs to Quests and even what items are available in shops!
This page is a list of Conds (Conditions), where they are found along with format documentation (*Yo-kai Watch 2 Conds DO NOT always work in other games i.e. Yo-kai Watch 3 - although they may. Please test this yourself*).

# Documented Useful Conds List
* Note: ***Some conds are customisable, in that case the raw bytes will be linked, use a website like [this](https://cryptii.com/pipes/hex-to-base64) to convert it to base64.***
* `0` - Always True / Null Cond. (Global)
  * `1` - Used in place of a Cond in certain situations. (Yo-kai Watch 2)
* `AAAAAA8FNRCxQJYAAQAyAAAAAXg=` - Main Story Completion (Yo-kai Watch 2). Obtained from Jungle Hunter (`shop_shopT001_0.01n.cfg.bin`)
* `00 00 00 00 0f 05 35 dd 77 26 95 00 01 00 32 00 00 00 XX 71` - Rank XX or higher Cond (Yo-kai Watch 2; replace XX with the rank, 00 = E, ... 05 = S). Obtained from Jungle Hunter (`shop_shopT001_0.01n.cfg.bin`).
* `00 00 00 00 18 05 35 8d 76 66 d8 00 0a 01 28 00 06 02 XX XX XX XX 00 32 00 00 00 01 78` - Has Item Cond. (Obtained from @z.u.ra on discord; replace XX XX XX XX with the ItemID in hex - remember to place the bytes from right to left).
  * Level5Condition.exe can also generate this Cond.

 # Conds Documentation/Specification

## 1. **Cond Overview**

A **Cond** is a little-endian binary structure representing a **boolean condition** in Yo-kai Watch games.
It can check things like flag states, inventory items, watch rank, or story progress.
* **Note: For the sake of simplicity and ease of access the hex here will be big-endian. The linked tool handles this appropriately.**
* Stored as a Base64 string in `cfg.bin`s. Convert to hex before handling.
* Can include **nested or extended parameter blocks**.
* Always ends with a **terminator** (`0x78` or `0x71`)??.


## 2. **Basic Cond Layout**
* Note: Examples are at the end.
```c
[HEADER 4B]
[OPCODE 2B]
35                    ; Section Start
[RESOURCE ID 4B]      ; ???
[EXTENSION]            ; Usually 3B: 00 01 00 (or 00 0D 0A 01 for extended due to null padding)
[EXTENSION DELIMITER]  ; 28 (optional, separates extension header from parameters)
[CTYPE 3B]             ; Comparison type / selector
32                     ; Section End
[COMPARISON VALUE 4B]  ; Value to compare in hex (e.g., 1 = `true`, `1` or the equivalent float).
[TERMINATOR 1B]        ; End of cond
```


## 3. **Headers and Opcodes**

| Field  | Example       | Meaning (Theorized)                                   |
| ------ | ------------- | ----------------------------------------------------- |
| HEADER | `00 00 00 00` | Reserved / start of Cond                              |
| OPCODE | `0F 05`       | Compare >= (constant comparison)                      |
| OPCODE | `18 05`       | Flag comparison maybe >=??                            |

## 4. **Sections and Delimiters**

| Byte | Meaning                                                              |
| ---- | -------------------------------------------------------------------- |
| `35` | Section Start / block delimiter                                      |
| `28` | Extension delimiter?? (separates extension header from sub-params)   |
| `32` | Section End / block terminator                                       |

* **Multiple `35` markers** prove nested sections are actually a thing and are often used in more complex conds like flag state checks.

## 5. **Extension Block**

* Usually starts as `00 01 00` for a simple check.
* Extended mode uses `00 0A 01` - adding a 3rd seems to pad a `00` (null termination) for unknown reasons:
  * Proven by a weird variant which uses `00 0D 0A 01`:

  * First byte (`0D`) = extended flags count / special mode???????
  * Second byte (`0A`) = AND / OR / combinator (most likely)
  * Third byte (`01`) = standard
* **Flags may be added together bitwise** to construct the final check value - but this is weird and iffy.

## 6. **Resource IDs**
* Resource ID = 4 bytes identifying *something*.
* Example: `A1 7D 35 34` 
> Note: `34`......


## 7. **Comparison Value**
* Always 4 bytes after the final section ends (`32`).
* Indicates the value to compare against.
  * `00 00 00 01` -> `0x00000001` aka `1` or `true`.
  * Another example is `00 00 00 05` aka `5` or Watch Rank S.

## 8. **Terminator**
* Usually `0x78` or `0x71`.
* Marks the end of the Cond.

## Examples:

* Basic Conds:
  * Watch Rank >= XX (YW2) Cond:
     * `00 00 00 00 0f 05 35 dd 77 26 95 00 01 00 32 00 00 00 05 71`
     * `00 00 00 00` - Header.
     * `0f 05` - Opcode; most likely `constant >=`.
     * `35` - Section Delimiter.
     * `DD 77 26 95` - Resource ID.
     * `00 01 00` - Ctype (Basic).
     * `32` - Section End Delimiter.
     * `00 00 00 05` - Comparison Value in this case `0x00000005` (S) for Watch Rank S.
     * `71` - Basic Terminator.
  * Main Story Completed (YW2) Cond:
     * `00 00 00 00 0f 05 35 10 b1 40 96 00 01 00 32 00 00 00 01 78`
     * `00 00 00 00` - Header.
     * `0f 05` - Opcode; most likely `constant >=`.
     * `35` - Section Delimiter.
     * `10 b1 40 96` - Resource ID.
     * `00 01 00` - Ctype (Basic).
     * `32` - Section End Delimiter.
     * `00 00 00 01` - Comparison Value in this case `0x00000001` (1) for `true`.
     * `78` - Basic Terminator.

  * Advanced (State Conds):
