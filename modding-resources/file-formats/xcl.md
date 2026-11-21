---
title: XCL
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: true
---

# XCL

**Filetype:** Collision File<br/>
**File Extension(s):** `.xcl`<br/>
**Platform(s):** Nintendo 3DS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** 4 Byte Alignment<br/>
**String Encoding:** ASCII

`.xcl` (X CoLlision) is a proprietary collision format developed by Level-5.
Later games use a different engine (and therefore no longer `.xcl`) and earlier games used `.prm` (mesh) collisions.

XCL files have no strings or file signature (magic).
XCL files are compressed in their entirety, not on a per-struct basis, using L5CC.

## Versions

There are two known versions of XCL, referred to here as V1 and V2.
The rest of the documentation assumes the file is already decompressed.

To detect the version, read the `u16` at offset `0x00` of the decompressed data.
If it's `1`:
  - The file is V1

If it's `0` read the `u16` at offset `0x02`.
  - If it's one, then the file is V2
  - If it's 0, it's an empty V1.

Neither are version fields; this works by coincidence.

## V2

A V2 XCL is structured like so:

| V2 XCL |
|---|
| Header |
| ChunkTable |
| Chunks |
| RawData |
| SegmentData* |

\*Optional: only present when the file has a TypeID 5 chunk.

### Header

The header contains offsets to each section of the file. Unlike V1, it does not point directly to data such as vertices; it defers that to later sections, in a way that is both forwards and backwards compatible. Even though, they did not use XCL for long enough to significantly benefit from this in practice, this is a much better design in theory.

```cpp
struct XCLV2Header {
    u16 reserved; // 0
    u16 dataCount; // 1 (or 0 in empty XCL files)
    u32 dataSize; // fileSize - 0x18; number of bytes excluding the header (0 in empty XCL files)
    u32 rawDataOffset; // absolute offset where raw data begins
    u16 chunkTableOffset; // 0x18; offset of the ChunkEntry array
    u16 chunkEntrySize; // 0x0C; size of a ChunkEntry
    u16 chunkCount; // number of ChunkEntry entries
    u16 chunkDataOffset; // 0x18 + 12 * chunkCount; absolute offset to the first chunk
    u32 chunkDataEnd; // rawDataOffset
};
```

### Chunk Table

The table begins at `chunkTableOffset` (always `0x18`). Each ChunkEntry declares the type, offset and size of a chunk, so a reader/writer can simply ignore types it has not been programmed for. This is what provides the compatibility mentioned above.

The array is sorted by `TypeID` in ascending order. In practice it is always exactly either `[1, 2, 3, 4]` or `[1, 2, 3, 4, 5]`, with contiguous `chunkOff`s of `0`, `12`, `32`, `40`, `92`.
It is unknown whether the ordering is a requirement, although this is rather likely. I have not checked, although this should be trivial to confirm.

All offsets here are relative to `chunkDataOffset`.

```cpp
struct ChunkEntry {
    u16 typeID; // please see later sections for the content of each TypeID
    u32 chunkOff; // relative offset from chunkDataOffset
    u16 reserved;
    u32 chunkSize; // size of the chunk in bytes
};
```

The absolute offset of a chunk is therefore `chunkDataOffset + chunkOff`.

### Chunks

All non-empty files contain TypeIDs 1 through 4. TypeID 5 only appears in select maps.

#### TypeID 1

TypeID 1 contains the basic geometry, meaning, the vertices and triangles.

```cpp
struct XCLV2Type1 {
    u32 reserved;
    u32 vertexBufferSize; // vertexCount * 12
    u16 vertexCount;
    u16 triCount;
};
```

#### TypeID 2

TypeID 2 contains grid data. All offsets in this block are relative to `rawDataOffset`.

```cpp
struct XCLV2Type2 {
    u32 geometrySize; // vertexCount * 12 + triCount * 12
    u32 remapArrayOffset;
    u32 zoneTableOffset;
    u16 surfaceTagCount;
    u16 remapRowCount; // row count covered by the remap array and zone table
    u32 zoneCount; // number of SurfaceZone entries
};
```

#### TypeID 3

TypeID 3 contains CollAttr data. All offsets in this block are relative to `rawDataOffset`.

```cpp
struct XCLV2Type3 {
    u32 collAttrDescCount;
    u32 collAttrDescOffset;
};
```

#### TypeID 4

TypeID 4 contains acceleration data. All offsets in this block are relative to `rawDataOffset`.

```cpp
struct XCLV2Type4 {
    u32 collAttrDescEnd;
    u32 runSlotCount; // also equal to the number of non-sentinel cells
    u32 cellGridOffset;
    u32 cellCount; // cellCols * cellRows
    u32 triIndexListOffset;
    u16 triIndexCount; // consumed length of the tri-index list
    u16 cellCols; // cells per grid row
    u32 cellRows;
    f32 aabbMinX;
    f32 aabbMinZ;
    f32 aabbMaxX;
    f32 aabbMaxZ;
    f32 cellSizeX; // ~ (aabbMaxX - aabbMinX) / cellCols (equal about 90% of the time)
    f32 cellSizeZ; // ~ (aabbMaxZ - aabbMinZ) / cellRows
};
```

#### TypeID 5
TypeID 5 is linked to map segmentation; all offsets in this block are relative to `rawDataOffset` unless stated otherwise.

```cpp
struct XCLV2Type5 {
    u32 segmentOffset;
    u16 segmentCount;
    u16 attrRefCount; // total per-segment collAttr references (aka the sum of the per-segment counts)
    u16 reserved;
    u16 hashArraySize; // 4 * segmentCount
    u32 countsOffset; // offset (from SegmentData start) of the count array
};
```

See [SegmentData](#segmentdata) for how these fields are used.

### V2 Raw Data Layout

All data begins at `rawDataOffset`.

#### Region order

Offsets are relative to `rawDataOffset` (V2) or `0x60` (V1).

| Region |
| --- |
| Vertex buffer |
| Triangle buffer |
| SurfaceTagEntry table |
| Remap array |
| *(4-byte alignment pad)* |
| Surface zone table |
| CollAttrDesc array |
| Run slots |
| Cell grid |
| Triangle-index list |
| *(4-byte alignment pad)* |
| SegmentData* |

\*Only for V2 XCL files with a TypeID 5 chunk.

#### Vertex buffer
```cpp
struct Vec3 {
    f32 x, y, z;
};
```

#### Triangle buffer

```cpp
struct Triangle {
    u16 v0, v1, v2;
    u16 zoneIndex;
    u16 collAttrIndex;
    u16 pad;
};
```

#### SurfaceTagEntry table

The table is a list of (tag, value) pairs - a palette per se.
A tag hash can appear many times with different values (for instance `t101g00` has ten `MINMAPNAM` entries and six `SOUND` entries); the entries a triangle actually uses are decided by the remap array below.

```cpp
struct SurfaceTagEntry {
    u32 tag; // CRC-32B hash of the collision type name
    u32 value;
    /*
     * the meaning of value is of course, dependent on the tag itself, for example:
     *  - SOUND value holds the footstep-sound to switch to on contact
     *  - MINMAPNAM contains the sublocation number to switch to on contact;
     *    - CRC32(<MAP>_mmn_%02d) is the TextID for the name in system text
     *  - RALCAM holds the camera to switch to on contact
     */
};
```

Hashes throughout the format, just like all other Level-5 file formats of this era, are CRC-32B (ZLib/ISO-HDLC) meaning:
  - Reflected polynomial `0xEDB88320`
  - Init and final XOR `0xFFFFFFFF`

And as you'd expect the null terminator is *not* included. Here are all the `tag`s:

```js
SOUND, SHADOW, RALCAM, CHRTYP, wal, MINMAPNAM, fltg, MESHCULL, CMRCOLON, ENV, MESHON, 0xC7CB7332
```

#### Remap array

The remap array contains `remapRowCount` u16s, where each is an index into the `SurfaceTagEntry` table; each entry is a row, and can be thought of as a pointer to a `(tag, value)` pair.

Rows are grouped into zones by the surface zone table below. A zone never has two rows with the same tag, so every tag appears at most once per zone.

For example, a zone made of three rows might resolve to `SOUND=2`, `CHRTYP=68`, `wal=1`. Any triangle that belongs to that zone gets all three of those settings.

> [!NOTE]
> Level-5 nearly always uses this order in a zone: SOUND comes first, usually followed by CHRTYP, MINMAPNAM, SHADOW, and RALCAM.

#### Surface zone table

`zoneCount` entries of 4 bytes each.

```cpp
struct SurfaceZone {
    u16 firstRow;
    u16 rowCount;
};
```

Note that `firstRow[i] + rowCount[i] == firstRow[i + 1]` and `sum(rowCount) == remapRowCount`.

Each SurfaceZone groups `rowCount` consecutive rows of the remap array into one zone, and `Triangle.zoneIndex` selects the zone a triangle belongs to.
For example, a zone with the pairs `SOUND=2`, `CHRTYP=68`, and `wal=1` is three rows wide.

<a name="colldesc"></a>

#### CollAttrDesc array

```cpp
struct CollAttrDesc {
    u32 nameHash; // <MAP>cl.<MESH>/<MAP>_racecl.<MESH> e.g. t101g00cl.hdcl01- or t108g00_racecl.cc01-
    u32 groupHash; // <MESH> e.g. hdcl01-
    u32 meshClass; // (may instead be u16 meshClass + u16 reserved?)
};
```

This is indexed into by `Triangle.collAttrIndex`.

#### Run slots

There is one 16-byte run slot per non-sentinel cell, in cell order. A cell's triangle list is split into up to four consecutive runs, one per run group; the slot records where each run is in the list.

```cpp
struct Span {
    u16 start;
    u16 end; // inclusive
}; // empty = {0xFFFF, 0xFFFF} (reads as 0xFFFFFFFF as a u32)

struct RunSlot { // 16 bytes
    Span floorRuns[2]; // run groups 0 and 1
    Span wallRuns[2]; // run groups 2 and 3
};
```

Span positions are relative to the cell's own slice of the triangle-index list, so `start <= end < listCount`.

##### Run Groups

Every triangle belongs to a singular run group, which determines which span of the slot it falls in:

| Group ID | Name      | Slot field      |
|----------|-----------|-----------------|
| 0        | floorRun1 | `floorRuns[0]`  |
| 1        | floorRun2 | `floorRuns[1]`  |
| 2        | wallRun1  | `wallRuns[0]`   |
| 3        | wallRun2  | `wallRuns[1]`   |

The group isn't stored in the actual triangle, but it is determined by the triangle's collision attribute (note that the same triangle is always in the same group, in every cell it appears in!)

The cell's triangles are concatenated in group order 0, 1, 2, 3, and each group is sorted by global triangle index. The spans are the boundaries between these parts.

A group with no triangles is `{0xFFFF, 0xFFFF}`.

Take for instance, a cell with 7 triangles, of which 3 are in group 0 and 4 are in group 2:

```js
floorRuns[0] = {0, 2}
floorRuns[1] = {0xFFFF, 0xFFFF}
wallRuns[0] = {3, 6}
wallRuns[1] = {0xFFFF, 0xFFFF}
```

#### Cell grid

`cellCount` records of 8 bytes each:

```cpp
struct Cell {
    u16 listStart; // start index into the triangle-index list
    u16 listCount; // 0 for sentinel cells
    u16 runSlotIdx; // index of this cell's run slot
    u16 _pad; // 0x0000 | 0xFFFF
};
/*
 * a sentinel cell is:
 * { uninitialized junk?, 0, 0xFFFF, 0xFFFF }
 */
```

The grid divides the AABB into `cellCols` by  `cellRows` equal rectangles, these are called tiles.

Cells are stored in row-major order; in short, cell `i` therefore refers to the tile at:

```js
tx = i % cellCols
tz = floor(i / cellCols)
```

A tile covers `[aabbMinX + tx * W / cellCols, aabbMinX + (tx + 1) * W / cellCols]` on X and the equivalent range on Z, whereby `W = aabbMaxX - aabbMinX` and the Z span is `aabbMaxZ - aabbMinZ`. The only tiles which no triangle refers to are sentinel cells.

The stored `cellSizeX` / `cellSizeZ` only approximately match `W / cellCols` (equal ~90% of the time).

A triangle is listed in every tile that its XZ projection makes contact with. Y is meaningless here, and boundaries are closed (aka touching a tile's edge counts). A large triangle therefore appears in many cells, and a cell can list triangles whose centers are in other tiles.

To rebuild a cell list, test each triangle's XZ projection against each tile rectangle with an exact 2D overlap test (separating axis theorem) in float64.

Although, I should mention *one* exception I've noticed:

The original writer sometimes lists a few *extra* triangles that miss a tile by usually at most ~0.0139 units (although there are exceptions ~15 units). Never fewer.
This mostly affects triangles with an edge nearly parallel to a tile boundary so a floor edge 0.0078 below a row boundary, for example, brings in the whole adjacent row of tiles.
Perpendicular near-misses are always excluded correctly. This looks like a float32 scanline rasterizer artifact in the original writer (?), although I haven't tried to perfectly mimic this when creating custom `.xcl`s, nor have looked into it much.

#### Triangle-index list

A flat `u16` array of triangle indices, located at `triIndexListOffset`. Each non-sentinel cell references a range of it.
The ranges together cover the first `triIndexCount` entries, so `triIndexCount == max(listStart + listCount)` over all non-sentinel cells.
The region contains `triIndexCount + 2 * cellCount` entries; the entries after the first `triIndexCount` are unused and zero-filled.
The region is then padded with `0x00` to a 4-byte file boundary (see [padding](#padding)).

<a name="padding"></a>

#### Padding

Both V1 and V2 follow these rules, and I've decided to give them a section of their own since I want to make sure nobody accidentally forgets to account for padding.

- All region offsets are 4-byte aligned, and regions follow each other with no gap (aside from the two pads below).
- The remap array is `remapRowCount` `u16` entries, and the zone table must start 4-byte aligned, therefore if `remapRowCount` is odd, 2 bytes of `0x00` are inserted between them; if it is even, there is no gap needed.
- After the triangle-index list region, the file is padded with `0x00` to a 4-byte boundary, so every file's size is a multiple of 4. Since the region is made of `u16` entries, if there is an odd amount, 2 byte padding is again needed.

The only padding that is not `0x00` is inside [SegmentData](#segmentdata), which is described there.

#### SegmentData

Present only in files with a TypeID 5 chunk, so never in V1. It begins at `rawDataOffset + Type5.segmentOffset` and runs to the end of the file.

```cpp
struct SegmentData {
    u32 segmentNameHash[segmentCount]; // CRC-32B of each segment's area code, in ascending order of hash
    u16 collAttrIdx[attrRefCount]; // every segment's attribute list concatenated
    u16 pad; // only present if attrRefCount is odd; probably uninitialized junk
    u16 attrCount[segmentCount]; // how many entries of collAttrIdx each segment has
    u16 tailPad; // only present if segmentCount is odd; probably uninitialized junk
};
```

`attrCount` starts at `Type5.countsOffset` bytes from the start of SegmentData, you don't have to compute where the padding ends.

Each hash names a map segment, a named sub-area of the map. Segmented maps are stored as a merged XCL for the whole map. Segment `i` holds the next `attrCount[i]` entries of `collAttrIdx`, starting where segment `i - 1` stopped as you'd expect.
Each entry is an index into the [CollAttrDesc](#colldesc) array.

For example, `t109d30` (YW3) has four segments with counts `[12, 3, 4, 3]`:

```js
segment 0: [7, 8, 9, 20, 21, 22, 33, 34, 35, 48, 49, 50]
segment 1: [53, 54, 55]
segment 2: [6, 19, 32, 47]
segment 3: [52, 56, 57]
```

##### Segment names

Each hash is the CRC-32B of an area code of the form `[A-Z]{1,3}[0-9]{0,2}`. There's not many (57) so I've listed them all below:

```yml
P01  P02  P03  P04  P05  P06  P07  P08  P10  P11  P21
PY1  PY2  PY3  PY4  PY5
Q01  Q10
ZN1  ZNT
L01
LC1  LC2  LC3
LO1  LO2  LO3
GY1  GY2  GY3  GY4
NK1  NK2
S01  S02  S03  S04  S05  S06  S07  S08
JP2
SH1  SH2
ORG
CIR
CR1
DOF
DON
E01
FES
BQ1
G01
R01
Y01
YS1
Z00
```

---

## V1

V1 instead has all the metadata placed into a single flat 0x60-byte header with hardcoded offsets. There are no TypeID blocks, nor is there a metadata entry table.

### Header

All offsets stored in the header are relative to `0x60` (end of header).

```cpp
struct XCLV1Header {
    u16 dataCount; // 1 (or 0 in empty files)
    u16 zero;
    u32 dataSize; // fileSize - 0x60 (testmap2 is an exception here; ignored for obvious reasons)
    u32 reserved;
    u32 triBufferOffset; // vertCount * 12
    u16 vertCount;
    u16 triCount;
    u32 geometrySize; // vertCount * 12 + triCount * 12
    u32 remapArrayOffset;
    u32 zoneTableOffset;
    u16 surfaceTagCount;
    u16 remapRowCount;
    u16 zoneCount;
    u16 collAttrDescCount; // should equal (collAttrDescEnd - collAttrDescOffset) / 12
    u32 collAttrDescOffset;
    u32 collAttrDescEnd;
    u32 runSlotCount; // also equal to the amount of real cells (excluding sentinels)
    u32 cellGridOffset;
    u32 cellCount; // cellCols * cellRows
    u32 triIndexListOffset;
    u16 triIndexCount;
    u16 cellCols;
    u32 cellRows;
    f32 aabbMinX;
    f32 aabbMinZ;
    f32 aabbMaxX;
    f32 aabbMaxZ;
    f32 cellSizeX; // ~ (aabbMaxX - aabbMinX) / cellCols (its exactly correct ~90% of the time) 
    f32 cellSizeZ; // ~ (aabbMaxZ - aabbMinZ) / cellRows; (same note as above ^ )
};
```

### V1 Raw Data Layout

Identical to V2 (including the [region order](#region-order) and [padding](#padding)), except with two changes: offsets are relative to the hardcoded offset `0x60` since there is no `rawDataOffset`, and there is no SegmentData.
