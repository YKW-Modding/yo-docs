---
title: XCL
parent: File Format Specifications
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

`.xcl` (X CoLlision) is a proprietary collision format developed by Level-5.
Later games use a different engine (and therefore no longer `.xcl`) and earlier games used `.prm` (mesh) collisions.

XCL files have no strings or file signature (magic).
XCL files are compressed in their entirety, not on a per-struct basis, using L5CC.

> [!CAUTION]
> Floats in this file should *never* be NaN, Infinity or -Infinity, although they can be -0.

## Versions

There are two known versions of XCL, referred to here as V1 and V2. V1 was introduced in Yo-kai Watch Blasters, while V2 was introduced in Yo-kai Watch 3.
The rest of the documentation assumes the file is already decompressed.

To detect the version, read the `u16` at offset `0x00` of the decompressed data.

If it's `1`: the file is V1.

If it's `0` read the `u16` at offset `0x02`.
  - If it's one, then the file is V2.
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

The header contains offsets to each section of the file. Unlike V1, it does not point directly to data such as vertices; it defers that to later sections, in a way that is both forwards and backwards compatible. Even though they did not use XCL for long enough to significantly benefit from this in practice, this is a much better design in theory.

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
#pragma pack(push, 1)
struct ChunkEntry {
    u16 typeID; // please see later sections for the content of each TypeID
    u32 chunkOff; // relative offset from chunkDataOffset
    u16 reserved;
    u32 chunkSize; // size of the chunk in bytes
};
#pragma pack(pop)
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

TypeID 2 contains surface metadata, including surface tags, remapping, and surface zones. All offsets in this block are relative to `rawDataOffset`.

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

TypeID 4 contains grid and acceleration data, including cells, run slots, and the triangle-index list. All offsets in this block are relative to `rawDataOffset`.

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

The grid divides the AABB into `cellCols` by `cellRows` equal rectangles, these are called tiles.

Cells are stored in row-major order; in short, cell `i` therefore refers to the tile at:

```js
tx = i % cellCols
tz = floor(i / cellCols)
```

Tile bounds are computed from the stored cell size, not by dividing the AABB. Bound `k` on X is `aabbMinX + k * cellSizeX`, so tile `tx` covers `[bound(tx), bound(tx + 1)]`, and Z works the same way with `aabbMinZ` and `cellSizeZ`. The outer edge of the last column and row is clamped to the AABB maximum. The only tiles which no triangle refers to are sentinel cells.

The stored `cellSizeX` / `cellSizeZ` only approximately match `(aabbMaxX - aabbMinX) / cellCols` and `(aabbMaxZ - aabbMinZ) / cellRows` respectively (equal ~90% of the time); the exact arithmetic is described below.

A triangle is listed in every tile that its XZ projection makes contact with. Y is meaningless here, and boundaries are closed (aka touching a tile's edge counts). A large triangle therefore appears in many cells, and a cell can list triangles whose centers are in other tiles.

Level-5 updated the tool that writes these lists somewhere after Yo-kai Watch Blasters and early into Yo-kai Watch 3 (I believe early since `testmap2` in YW3 uses the YWB code, being the only file that does this).

> [!NOTE]
> To clarify, as implied above, the writer is distinguished by game *NOT* version. V1 YW3 `.xcl` files use YW3's code not YWB's.

Both writers work on a per triangle basis. The tiles under the triangle's bounding box are found first, and that range is extended by one tile so that tiles the box only touches are also tested. Each tile in that range is then checked against the triangle with a separating axis test (on the X and Z axes and the three edge normals), where touching does indeed count as overlap.

Zero-length edges are skipped, and the last column and row clamp their outer edge to the AABB maximum. YW3's writer has an `eps` tile inflation of `0.01` on all sides, YWB has none. YWB uses `f32(aabbMin + k * cellSize)` for tile bound `k`, while YW3 uses `aabbMin + k * cellSize` (as float64).

YW3's inflation only adds triangles - it can't remove any. On the X and Z axes, a gap of up to `eps` still counts as overlap. On an edge axis with (unit) normal `(nx, nz)`, the inflated tile's projection is wider by `eps * (abs(nx) + abs(nz))` at each end, so the allowed gap depends on the edge's angle. At most, it can reach `eps * sqrt(2)` (~0.0141) for a 45 degree edge.

> [!NOTE]
> In case you were wondering, the `0.01` inflation appears very much intentional.
> 
> As for why, I believe the grid is a broadphase, so it just needs to never miss a collider. Listing a triangle in a tile it barely grazes really only leads to an extra check at runtime. However, leaving it out could let something slip through a seam, and Yo-kai Watch had enough OOB glitches.
> Take a typical `cellSize` of ~39.4. In this case, `0.01` is about 0.03% of a tile, so it costs next to nothing in practice anyway.

Because YWB's float32 bounds sit a few millionths of a unit off the float64 grid, a triangle that only touches a tile can land on either side; this is the only case where a list has fewer triangles than an exact test would give. There are exactly 39 such occurrences in YWB.

The stored `cellSizeX` / `cellSizeZ` actually depends on the format instead of the game. V2 files compute it entirely in float32 (so `f32(f32(span) / count)`), while V1 files divide the double span and only round the quotient, so `f32(span / count)`.

The two usually yield the same result. They only differ if the span isn't exactly representable in float32, the count isn't a power of two (as dividing by a power of two is just an exponent shift), and the quotient sits close enough to a rounding boundary that rounding the span first tips it over. When they do differ it's always by exactly 1 ulp.

Because of the conditions above, only a minority of files can actually show which one was used, but all of them follow the version. All 106 decidable V2 files use the two-step form, and all 15 decidable V1 files use the single-rounded one. That includes YW3's V1 files, even though they use YW3's tile code. There isn't a single file which mixes the two between its axes.

Therefore, I conclude Level-5 likely changed the tile code first, and the cell size arithmetic later, around when V2 was introduced. I doubt the cell size change was deliberate. A 1 ulp difference is probably just whatever width the span was held in by each build.

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

Each hash is the CRC-32B of an area code of the form `[A-Z]{1,3}[0-9]{0,2}`. There aren't many (57) so I've listed them all below:

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
    u32 dataSize; // fileSize - 0x60 (testmap2 is an exception here; ignored as it's an unused test map)
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
