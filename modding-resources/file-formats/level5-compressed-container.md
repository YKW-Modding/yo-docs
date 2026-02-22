---
title: Level5 Compressed Container
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: true
---

# Level5 Compressed Container

**Filetype:** N/A<br/>
**Platform(s):** Nintendo 3DS<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>
**Alignment:** N/A<br/>
**String Encoding:** N/A

The Level5 Compressed Container is a structure commonly used within Level5's proprietary formats to store compressed data (e.g., `ARC0`) as implemented by:
```cpp
void lxpUncompress(void *outBuffer, uint32_t& outDecompressedSize, uint32_t outBufferSize, const void *compressedContainer) {}
uint32_t lxpUncompress_Size(void *compressionHeader) {}
uint32_t lxpUncompress_Type(void *compressionHeader) {}
```
Nearly all compressed data is stored using this structure.

## Format Layout

```md
u32 compressionHeader
byte[] compressedBlob
```

### compressionHeader Layout

```cpp
compressionMethod = compressionHeader & 0x7
decompressedSize = compressionHeader >> 3
```
The lower 3 bits represent the compression method whereas the remaining (upper 29) bits hold the decompressed size in bytes.

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

After decompression, the resulting stream is used as the actual table data.

# Example Implementations
## LZ10
### JS
```js
/**
 * Decompresses a Uint8Array compressed via Nintendo's LZ10 algorithm.
 * 
 * @param {Uint8Array} compUint8 - Uint8Array of the compressed data.
 * @param {Number} expectedSize - Expected size of the decompressed output.
 * @returns {Uint8Array} - Decompressed output.
 */
function lxp_uncompress_lz10(compUint8, expectedSize) {
  if (!compUint8 || compUint8.length === 0) return new Uint8Array(0); // Early exit for empty input
  let inPos = 0;
  const input = compUint8;
  if (input.length >= 4) inPos = 4; // Skip compressionHeader
  
  const output = new Uint8Array(expectedSize); // pre-allocate a buffer for the output with size expectedSize
  let outPos = 0;

  while (outPos < expectedSize) {
    if (inPos >= input.length) break;

    // Read a flag byte at position inPos and jump past it where each bit can be a 0 (representing a literal) or a 1 (representing a compressed pair)
    const flag = input[inPos++];

    // Process each bit within the flag byte starting from the MSB (leftmost/largest bit) 
    for (let bit = 7; bit >= 0; bit--) { // for each bit
      if (outPos >= expectedSize) break;
      const isCompressed = (flag >> bit) & 1; // grab it

      if (isCompressed) { // and if it's 1 make sure there's enough bytes for a compressed pair
        if (inPos + 1 >= input.length) { // if not
          console.warn('LZ10: truncated compressed pair'); // warn
          break; // and skip
        }

        const [ b1, b2 ] = [ input[inPos++], input[inPos++] ];
        const length = (b1 >> 4) + 3; // the length of the repeated sequence can be calculated as the upper nibble + 3
        const disp = ((b1 & 0x0F) << 8) | b2; // get the displacement (distance back to copy from)
        let src = outPos - (disp + 1);
        if (src < 0) src = 0; // here I clamped src to zero so it can't accidentally read before the start of the buffer.

        // Copy the sequence from already decompressed data
        for (let k = 0; k < length && outPos < expectedSize; k++) output[outPos++] = output[src++];
      } else {
        // if it's 0 aka it represents a literal byte then copy it directly from I->O
        if (inPos >= input.length) { // safety because yes
          console.warn('LZ10: truncated literal');
          break;
        }
        output[outPos++] = input[inPos++]; // copy from input to output while advancing (did I spell that right - looks off) both outPos and inPos
      }
    }
  }

  return output;
}
```
## RLE
### JS
```js
/**
 * Decompresses a Uint8Array using Level5's RLE Implementation
 * @param {Uint8Array} compUint8 - Uint8Array of the compressed data.
 * @param {Number} expectedSize - Expected size of the decompressed output.
 * @returns {Uint8Array} - Decompressed output.
 */ 
function lxp_uncompress_rle(compUint8, expectedSize) { // works
  const [input, out] = [ compUint8, new Uint8Array(expectedSize)]; // preallocate an output buffer using expectedSize 
  let [inPos, outPos] = [ 0, 0 ]; // init ptrs

  while (outPos < expectedSize && inPos < input.length) {
    const flag = input[inPos++]; // read the byte @ inPos then advance inPos by a byte

    if (flag & 0x80) { // read the high bit to check if it's a repeat run or a literal run
      // if it's a repeat run then,
      if (inPos >= input.length) break; // (just to be safe lol)
      const val = input[inPos++]; // read the value @ inPos (the byte we're repeating) and advance the ptr
      const repetitions = (flag & 0x7F) + 3; // number of times to repeat next byte (val)
      const remaining = expectedSize - outPos;
      const count = Math.min(repetitions, remaining);
      for (let i = 0; i < count; i++) out[outPos++] = val; // loop count times filling count bytes with val @ outPos, while advancing outPos
    } else {
      // Literal run
      const length = flag + 1;
      const remaining = expectedSize - outPos;
      const count = Math.min(length, remaining);
      for (let i = 0; i < count; i++) { // loop over count times
        if (inPos >= input.length) break; // (checking for safety ofc)
        out[outPos++] = input[inPos++]; // and filling in count bytes from inPos to outPos, while advancing both ptrs ofc
      }
    }
  }
  return out;
}
```
