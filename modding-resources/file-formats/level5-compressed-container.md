---
title: Level5 Compressed Container
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: true
---

# Level5 Compressed Container

**Filetype:** N/A<br/>
**File Extensions:** N/A<br/>
**Signature:** N/A<br/>
**Platform(s):** Nintendo 3DS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** N/A<br/>
**String Encoding:** N/A

The Level5 Compressed Container is a structure used within Level5's proprietary formats to store compressed data (e.g., `ARC0`) as implemented by:
```cpp
void lxpUncompress(void *outBuffer, uint32_t& outDecompressedSize, uint32_t outBufferSize, const void *compressedContainer) {}
uint32_t lxpUncompress_Size(void *compressionHeader) {}
uint32_t lxpUncompress_Type(void *compressionHeader) {}
```
All compressed data is stored using this structure. This structure starts with a 32-bit header and then the raw compressed blob. The lower 3 bits hold the compression bits, while the remaining 29 bits hold the decompressed blob's size in bytes.

## Compression Methods

| ID | Method        |
| -- | ------------- |
| 0  | Uncompressed  |
| 1  | LZ10          |
| 2  | Huffman 4-bit |
| 3  | Huffman 8-bit |
| 4  | RLE           |
| 5  | ZLib          |
| 6  | Invalid       |
| 7  | Invalid       |

> Compression method IDs 6 and 7 are considered invalid and should not appear in a valid L5CC. Ideally treat these as errors.
