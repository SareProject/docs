---
title: Private Keys
layout: default
parent: Formats
---

## Private Keys

Private keys are essential components of encryption systems, used for decryption, signature generation, and key derivation. This document provides an overview of private keys, including their structure and encoding formats.

### SecretKeyFormat

- `ec_algorithm`: Specifies the elliptic curve algorithm used for private key generation.
- `pq_algorithm`: Indicates the post-quantum algorithm used for private key generation.
- `dh_algorithm`: Specifies the Diffie-Hellman algorithm used for private key generation.
- `kem_algorithm`: Indicates the Key Encapsulation Mechanism (KEM) algorithm used for private key generation.
- `master_seed`: A secret value used as the master seed for deriving other private keys.
- `encryption_metadata`: Additional metadata related to encryption, such as nonce and key derivation parameters. This field is present if the master seed is symmetrically encrypted.

```
+---------------------+
|   ec_algorithm      |
|   pq_algorithm      |
|   dh_algorithm      |
|   kem_algorithm     |
|   master_seed       |
|   encryption_metadata   |
+---------------------+
```

[Encryption Metadata Format](/Formats/encryption-metadata-format)
