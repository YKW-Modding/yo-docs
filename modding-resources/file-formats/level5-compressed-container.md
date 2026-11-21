---
title: Level5 Compressed Container
parent: File Format Specs
grandparent: Modding Resources
layout: default
has_children: true
---

# Level5 Compressed Container

**Platform(s):** All<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>

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

ZLib, although implemented in the versions of `lxp` used, is unused in the Yo-kai Watch franchise.

## Official Algorithms
Via analysis of compressed blobs, I have figured out the exact algorithms used by Level-5s tooling for producing the blobs (likely makes use of Nintendo code, although this is entirely clean-room on my end).

### Method Choice
This is by far the simplest. First, all algorithms (aside from Uncompressed and ZLib) are computed over the blob. The smallest one is picked, ties go to the algorithm with the lowest ID.
Next, if the finished blob (including the L5CC header) is no larger than the raw data, the winner is used, otherwise method 0 (Uncompressed) is used.

### Uncompressed (0)
Nothing.

### LZ10 (1)

The stream is a sequence of groups. Each group starts with a flag byte, read MSB-first, followed by up to 8 tokens, one per flag bit, note that a set bit marks a match token and a clear bit marks a literal token.
A literal token is a single byte which is copied as-is. A match token is two bytes, `b1` and `b2`, that copy `(b1 >> 4) + 3` bytes (3 to 18) from `(((b1 & 0xF) << 8) | b2) + 1` bytes back (1 to 4096). A match may be longer than its distance, in which case the copy overlaps itself and repeats the pattern.

The original encoder is greedy; at each position it takes the longest match available at distances up to 4096, considering only matches of at least 3 bytes and at most 18 bytes. A longer repeat is therefore covered by several consecutive matches. If several matches have the same length, it takes the farthest one. It never emits a match at distance 1; its search skips the immediately preceding byte entirely, so a run of one repeated byte whose first three bytes do not occur earlier in the window starts with two literals and is then covered by overlapping matches at distance 2 or more. A flag group is written only once it has at least one token, and unused flag bits in the last group are zero.

### Huffman-4 (2)

This is a complicated one.

After the block header, the stream is two header bytes, a tree, and a bit stream. `treeSize` (u8) is the number of 16-bit pairs in the tree, `treeRoot` (u8) is the root node's value, and `treeSize * 2` bytes of tree follow. Then comes the bit stream which is composed of 32-bit little-endian words whose bits are consumed MSB-first. The symbols are nibbles, and each output byte is decoded as two symbols, low nibble first.

The tree is a list of pairs numbered from 0. Pair `k` holds the two child values of one internal node: the bit-0 child in the first byte and the bit-1 child in the second.
A leaf child is stored as its bare symbol and an internal child is stored as a node value.
Bits 7 and 6 of a node value say whether that node's own bit-0 and bit-1 children are leaves.
Bits 0 to 5 are `c`, and the node's children are in pair `containing_pair + 1 + c`, where `containing_pair` is the pair holding this value.
The root has no containing pair, so `treeRoot` holds only its two leaf flags with `c = 0`, and its children are in pair 0.

The original encoder does the following, in order:

1. Counts nibble frequencies over both nibbles of every byte.
2. Seeds one node per used symbol in ascending symbol order, then repeatedly merges the two lightest nodes. The lighter becomes the bit-0 child, and on equal weight the node earlier in the list goes first. The merged node is appended to the end of the list. Leaves therefore always precede merged nodes, so on equal weight a leaf beats a merged node, leaves ordered by symbol and merged nodes by creation order.
3. Serializes breadth-first from the root. The `k`-th internal node visited gets pair `k`, and `c` follows from the formula above.
4. Pads the tree with one `{00 00}` pair if the pair count is even. The result is always odd, so the whole block stays 4-byte aligned.
5. Writes each byte's low-nibble code, then its high-nibble code, with code bits root to leaf (bit-0 branch = 0). Bits fill each 32-bit word from bit 31 down, the word is stored little-endian, and the last word is zero-padded.

### RLE (4)
The stream is a sequence of tokens, each introduced by a flag byte. If the top bit is set, the token is a run meaning the next byte is repeated `(flag & 0x7F) + 3` times (3 to 130).
Otherwise it is a literal meaning the next `flag + 1` bytes (1 to 128) are copied as-is.

The original encoder is greedy; at each position it takes the longest run of identical bytes starting there, and emits it as a run token only if it is at least 3 bytes long. Shorter runs (1 or 2 bytes) are appended to a literal buffer. The buffer is emptied, as literal tokens of (at most) 128 bytes each, in three scenarios: as soon as it holds 128+ bytes, just before the next run token, or if it's at the end of the input. Because a short run adds up to 2 bytes, the buffer can reach 129 bytes before the check, in which case it is flushed as a 128-byte token followed immediately by a 1-byte token; the leftover byte is therefore not held back.

A single token can't cover more than 130 repeats, so a longer run is cut at 130 and the leftover bytes of that run are scanned again from the cut point like any other input. For instance, 133 identical bytes become two run tokens of 130 and 3, while 131 become a 130-run + one literal.
