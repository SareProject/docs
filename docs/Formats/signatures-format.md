---
title: Signatures Format
layout: default
parent: Formats
---

## Signatures

Signatures play a crucial role in verifying the authenticity and integrity of messages. This document provides an overview of signatures, including their structure, encoding formats, and metadata.

### SignatureFormat

- `signature_metadata`: Metadata related to the signature, including the elliptic curve and post-quantum algorithms used.
- `ec_public_key`: Public key component for the elliptic curve algorithm.
- `pq_public_key`: Public key component for the post-quantum algorithm.
- `message`: The message that was signed.
- `ec_signature`: Signature generated using the elliptic curve algorithm.
- `pq_signature`: Signature generated using the post-quantum algorithm.

```
+-------------------------+
|   signature_metadata    |
|   ec_public_key         |
|   pq_public_key         |
|   message               |
|   ec_signature          |
|   pq_signature          |
+-------------------------+
```

### SignatureMetadataFormat

- `ec_algorithm`: Specifies the elliptic curve algorithm used for signature generation.
- `pq_algorithm`: Indicates the post-quantum algorithm used for signature generation.

## Encoding

The SignatureFormat is encoded to BSON format. Optionally, it can be further encoded to PEM format for ASCII representation.