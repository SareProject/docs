---
title: Revocation Certificate Format
layout: default
parent: Formats
---

### Revocation Certificate Format

Revocation certificates contain essential details regarding the revocation event. Below are the fields and their descriptions:

- `issuer`: Identifies the entity that issued the certificate.
- `revocation_date`: Specifies the date when the revocation occurred, represented as a Unix timestamp.
- `revocation_reason`: Provides the reason for the revocation, explaining why the public key is being revoked.

```
+-------------------------+
|       issuer            | 
|-------------------------|
|    revocation_date      |
|-------------------------|
|    revocation_reason    |
+-------------------------+
```
