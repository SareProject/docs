---
title: Encryption Metadata Format
layout: default
parent: Formats
---

## Encryption Metadata Format

The Encryption Metadata Format defines metadata related to encryption, including parameters used in key encapsulation and password-based key derivation.

### EncryptionMetadataFormat

- `encryption_algorithm`: Specifies the encryption algorithm used.
- `nonce`: Nonce used in encryption for achieving probabilistic encryption.
- `kem_metadata`: Metadata related to the Key Encapsulation Mechanism (KEM), including the KEM algorithm, Diffie-Hellman parameters, ciphertext, and salt.
- `pkdf_metadata`: Metadata related to the Password-Based Key Derivation Function (PKDF), including the salt and algorithm.

```
+---------------------+
|   encryption_algorithm  |
|   nonce               |
|   kem_metadata        |
|   pkdf_metadata       |
+---------------------+
```

[Key Encapsulation Mechanism Metadata Format](./kem-metadata-format)

[Password-Based Key Derivation Function Metadata Format](./pkdf-metadata-format)