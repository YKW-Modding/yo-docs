---
title: Level-5 Compressed Container
parent: File Format Specifications
grandparent: Modding Resources
layout: default
has_children: true
---

# Level-5 Compressed Container

**Platform(s):** All<br/>
**Endianness:** Little-endian<br/>
**Used in:** Yo-kai Watch 1, Yo-kai Watch 2, Yo-kai Watch Blasters, Yo-kai Watch 3, Yo-kai Watch Busters 2<br/>

The Level-5 Compressed Container is a structure used within Level-5's proprietary formats to store compressed data (e.g., `ARC0`) as implemented by:
```cpp
void lxpUncompress(void *outBuffer, uint32_t& outDecompressedSize, uint32_t outBufferSize, const void *compressedContainer) {}
uint32_t lxpUncompress_Size(void *compressionHeader) {}
uint32_t lxpUncompress_Type(void *compressionHeader) {}
```
All compressed data is stored using this structure. It starts with a 32-bit header and then the raw compressed blob. The lower 3 bits hold the compression method ID, while the remaining 29 bits hold the decompressed blob's size in bytes as an unsigned integer. Because of this, the maximum decompressed size is 536,870,911 bytes, or one byte under 512 MiB (not MB; these are different units!).

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

> [!WARNING]
> Compression method IDs 6 and 7 are considered invalid and should not appear in a valid L5CC. Ideally treat these as errors.
> 
> Furthermore, ZLib, although implemented in late versions of `lxp`, is unused in the Yo-kai Watch franchise. Because of its late implementation, several Yo-kai Watch games do not support it. For instance, Yo-kai Watch 2 does not.

## Official Algorithms
I have figured out the algorithms used by Level-5s tooling for producing the blobs (likely makes use of Nintendo code, although this is entirely clean-room on my end) via analysis of compressed blobs.

> [!IMPORTANT]
> To clarify, decoding rules describe the structure of valid L5CC data, while the encoding rules aim to reproduce the behaviour of Level-5's original tooling to the byte. An implementation does not necessarily need to reproduce the original encoder's compression choices (in fact, all tools made before this specification do not). However, doing so is useful for both round-trip verification of file formats, and for providing a general reference for how a good encoder should compress files.

### Method Choice
This is by far the simplest. First, all algorithms (aside from Uncompressed and ZLib) are computed over the blob. The smallest resulting stream is selected (note that ties go to the algorithm with the lowest ID).
Next, if the finished blob (including the L5CC header) is no larger than the raw data, the winner is used, otherwise method 0 (Uncompressed) is used.

### Uncompressed (0)
Nothing.

### LZ10 (1)
The stream is a sequence of groups. Each group starts with a flag byte, read MSB-first. This is followed by up to 8 tokens, one per flag bit, note that a set bit marks a match token and a clear bit marks a literal token.
A literal token is a single byte which is copied as-is. A match token is two bytes, `b1` and `b2`, that copy `(b1 >> 4) + 3` bytes (3 to 18) from `(((b1 & 0xF) << 8) | b2) + 1` bytes back (1 to 4096).
A match may be longer than its distance, in which case the copy overlaps itself and repeats the pattern.

The original encoder is greedy; at each position it takes the longest match available at distances up to 4096, considering only matches of at least 3 bytes and at most 18 bytes, as there aren't enough bits to represent longer matches. Therefore, such a match is handled by several consecutive matches. If several matches have the same length, it takes the farthest one. The algorithm never emits a match at distance 1; the search skips the immediately preceding byte entirely, so a run of one repeated byte whose first three bytes do not occur earlier in the window starts with two literals and is then covered by overlapping matches at distance 2 or more. A flag group is written only once it has at least one token, and unused flag bits in the last group are zero.

### Huffman-4 (2)

This is a complicated one.

After the block header, the stream consists of two header bytes, a tree, and a bit stream.
`treeSize` (u8) is the number of 16-bit pairs in the tree, and `treeRoot` (u8) is the root node's value. These are followed by `treeSize * 2` bytes of tree data.

So:
```hi
[u8: treeSize] [u8: treeRoot] [treeSize * 2: treeData] [bitStream]
```

Huffman decoding traverses the tree one bit at a time. Each bit of the bitstream determines which of the current node's two children to follow: a 0 follows the bit-0 branch, while a 1 follows the bit-1 branch. These branches are inside the tree data, which will be explained below:

The tree data is a list of pairs numbered from 0. Each pair represents an internal node and stores the values of its two children.
Every pair consists of two bytes; the first byte represents the child reached by following a bit-0 ("left") branch, while the second represents the child reached by following a bit-1 ("right") branch.

So, in total, assuming we are reading bit `n` of `bitStream`:

```c
pair = treeData->pairs[currentNode];
follow pair[bitStream[n]];
```

A child can either be a leaf or an internal node. Leaf children are stored as-is, meaning a leaf node is a literal nibble emitted into the decompressed blob. Internal children, however, are stored as node values.

A node value contains information about its own children. Bits 7 and 6 indicate whether its bit-0 and bit-1 children, respectively, are leaves. The remaining 6 bits, referred to as `c`, encode an offset used to locate the pair containing the node's children.

The pair containing a node's children is located at `containing_pair + 1 + c`, where `containing_pair` is the index of the pair containing the node's value. In other words, the target pair is `c` pairs after the containing pair.

The root is a special case here, as it has no containing pair. Its value is stored in `treeRoot`, which holds only the two leaf flags, with `c` being 0. Its children are always stored in pair 0.

The bitstream encodes the output as a sequence of 4-bit symbols (nibbles), using variable-length codes determined by the tree. Each output byte is reconstructed from two consecutive symbols, which provide the low and high nibbles respectively.

The encoded bits are stored in 32-bit little-endian words, with bits consumed MSB-first within each word.

Now, for the encoder rules. Perform the following steps, in order, to mimic the original encoder:

1. Count the frequency of each nibble across both nibbles of every input byte.
2. Create one node for each used symbol, in ascending symbol order. Then repeatedly merge the two nodes with the lowest weights. The node with the lower weight becomes the bit-0 child, while the other becomes the bit-1 child. If their weights are equal, the node appearing earlier in the list should be chosen first. Each merged node is appended to the end of the list. Because of this, leaves always precede merged nodes, and ties are resolved by symbol order for leaves and creation order for merged nodes.
3. Serialize the tree in breadth-first order, starting from the root. Each internal node is assigned a pair index in visitation order, starting at 0. The `c` value is then calculated using the formula described above.
  - If the number of tree pairs is even, append two bytes of padding (all-zeros!). This ensures that the entire block is 4-byte aligned.
4. Finally, encode each input byte as two consecutive symbols: its low nibble and its high nibble, in that order. Each symbol's Huffman code is written from root to leaf, with bit-0 branches represented by `0` and bit-1 branches by `1`. Bits fill each 32-bit word from bit `31` downwards. Each word is stored in little-endian order as one would expect. The final word is zero-padded if necessary.

### Huffman-8 (3)

This will also be a long chain of prose, get ready.

Huffman-8 is Huffman-4 with byte symbols instead of nibbles. The differences are as follows:

- Frequencies are counted per byte.
- Every input byte is a single symbol, and each symbol likewise emits a single byte of output.
  - There is no nibble ordering.
- The tree serialization changes based on which pair is assigned to which internal node (but not the resulting Huffman codes or their lengths!).

Therefore, decoders require no changes to their tree traversal logic from Huffman-4 to Huffman-8.

#### Tree Serialization

Plain breadth-first serialization, as used by Huffman-4, does not always work for Huffman-8.
In wider trees, the distance between an internal node and the pair containing its children can exceed the maximum representable by the `c` field.
Trees with up to 65[\*](#sadnote) internal pairs use the Huffman-4 layout. Larger trees use a different layout, which I will refer to as the deadline scheduler.

For this layout, each internal node holds the pair containing its children's values, while the root always holds pair 0. `np` tracks the next unassigned pair, starting at 1.

The frontier is an ordered list of internal nodes whose parent already holds a pair, but which have not yet been assigned one themselves.
The frontier is (initially) populated with the root's internal children, in bit-0 then bit-1 order. Leaf nodes do not ever enter the frontier.

Each frontier node has a deadline, defined as its parent's pair index + 64. This is the last pair index it can occupy while keeping the offset to its children's pair within the range representable by `c`.

`size(x)` is the number of internal nodes in `x`'s subtree, including `x` itself.

The scheduler repeatedly performs the following procedure until the frontier is empty:

1. Schedule a subtree. Scan the frontier newest-first and select the first node `x` that fulfils the following criteria:

  - Let `end = np + size(x)`.
  - Sort the deadlines of all frontier nodes in ascending order, including x's deadline, giving `dl[0] <= dl[1] <= ...`.
  - `end + i` must be <= `dl[i]` for every index `i`, starting at 0.

If a node passes, remove x from the frontier and serialize its entire subtree as a plain breadth-first block, visiting bit-0 children before bit-1 children.
The block occupies pairs `np` through `end - 1`, with `x` being assigned pair `np`. Then set `np = end`.

2. Expand the most urgent node. If no frontier node passes the above test, select the node with the smallest deadline. Ties are broken by larger subtree size, then by position in the frontier (earliest wins!).

Assign this node pair `p = np`. Append its internal children to the frontier, in bit-0 then bit-1 order, each with a deadline of `p + 64`. Finally, increment `np` by one.
> No, not that P=NP :p

Once every internal node has been assigned a pair, node values are encoded using the same formula as Huffman-4.

The scheduling criteria include x's own deadline, therefore, in effect, requiring an additional available slot more than what placing x technically requires.
Therefore, since sometimes no candidate passes even though a subtree could, in theory, be placed without violating a deadline, one may consider it to not be a feasibility check per se.

> [!NOTE]
> <span id="sadnote">\*</span>There are some unknowns; namely: the 65-pair threshold is actually just a guesstimate on my end as no trees containing between 16 and 252 internal pairs have been checked, so the actual threshold could, in theory, be anywhere within that range. Additionally, the tree representation for inputs containing fewer than two distinct symbols is unknown, as such inputs should always select another compression method, although this is useless information in practice.

### RLE (4)
The stream is a sequence of tokens, each introduced by a flag byte. If the top bit is set, the token is a run meaning the next byte is repeated `(flag & 0x7F) + 3` times.
Otherwise it is a literal meaning the next `flag + 1` bytes are copied as-is.

The encoder used by Level-5 is a greedy encoder; at each position it takes the longest run of identical bytes starting there, and emits it as a run token only if it is at least 3 bytes long, as lower sequences can't be represented this way. Shorter runs are therefore appended to a literal buffer. The buffer is emptied, as literal tokens of (at most) 128 bytes each (as a literal flag cannot represent > 128 bytes), once one of three scenarios occur:

- Once it holds 128 or more bytes[\*](#twonote)
- If it is followed by a run token.
- If EOF has been reached.

> [!IMPORTANT]
> <span id="twonote">\*</span>Because a short run adds up to 2 bytes, the buffer can reach 129 bytes before the check, in which case it is flushed as a 128-byte token followed immediately by a 1-byte token; the leftover byte is therefore not held back.

Because a single token can't cover > 130 repeats, a longer run is therefore cut at 130 and the leftover bytes are scanned again from the cut point like any other input.
For instance, 133 identical bytes become two run tokens of 130 and 3, while 131 becomes a 130-run + a literal.

### ZLib (5)
The compressed stream is a standard ZLib-wrapped DEFLATE stream, as defined by [RFC 1950](https://www.rfc-editor.org/rfc/rfc1950). Decompression is performed using zlib's `inflate` API in Level-5 games which support this method. As implied above, the stream includes the standard ZLib header and trailing Adler-32 checksum. The decompressed size is provided by the L5CC header.

> [!CAUTION]
> Many Yo-kai Watch games do NOT implement ZLib. Only use ZLib in supporting games such as Yo-kai Watch 1 for Smartphone.
