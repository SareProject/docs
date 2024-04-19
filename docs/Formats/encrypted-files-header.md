---
title: Header Format for Encrypted Files
layout: default
parent: Formats
---

## Header Format for Encrypted Files

When Sare encrypts a file, it adds a header at the beginning to keep everything organized. Here's how it looks:

### Magic Bytes

At the very start, there are some special letters that show it's a Sare-encrypted file.

- **Magic Bytes:** `CRYPTOPIA`

### Header Structure

The header has a few parts:

1. **Version (4 bytes):** This is just a number that shows which version of encryption Sare used.

2. **Metadata Length (8 bytes):** This number tells how long the metadata section is.

3. **Metadata (variable length):** This part has info about how the file was encrypted. It includes stuff like what method was used to hide the key and if there's a signature.

4. **Signature Length (8 bytes):** If there's a signature, this number says how long it is. If there's no signature, it's just 0.

5. **Signature (variable length):** If there's a signature, it's right here. It's like a stamp that proves the file is real.

### Encoding and Decoding

#### Encoding

To put together the header:

1. Start with the magic bytes `CRYPTOPIA`.
2. Add the version number.
3. Write down how long the metadata part is.
4. Write the metadata.
5. Say how long the signature part is (if there's one).
6. Write the signature.

#### Decoding

To understand the header:

1. Check if it starts with `CRYPTOPIA`.
2. Look at the version number.
3. See how long the metadata part is, and read it.
4. Check if there's a signature and read it if there is.

### Schematics

Here's a simple schematic to visualize the structure of the header:

```
+-----------------+--------+-------------------------------------------+--------+-------------------------------------------+
| Magic Bytes     | Version| Metadata Length | Metadata        | Sig Len| Signature                                  |
| (CRYPTOPIA)     | (4 bytes)          | (8 bytes)             |            | (8 bytes)            |                                            |
+-----------------+--------+-------------------------------------------+--------+-------------------------------------------+
```
