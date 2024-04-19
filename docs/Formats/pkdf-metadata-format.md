---
title: Password-Based Key Derivation Function Metadata Format
layout: default
parent: Formats
---

## Password-Based Key Derivation Function Metadata Format

The Password-Based Key Derivation Function (PKDF) Metadata Format defines metadata related to the Password-Based Key Derivation Function used in encryption.

### PKDFMetadataFormat

- `pkdf_salt`: Salt used in the Password-Based Key Derivation Function (PKDF).
- `pkdf_algorithm`: Specifies the Password-Based Key Derivation Function (PKDF) algorithm used.

```
+----------------------+
|   pkdf_salt          |
|   pkdf_algorithm     |
+----------------------+
```