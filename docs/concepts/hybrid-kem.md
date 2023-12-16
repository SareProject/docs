---
title: Hybrid KEM
layout: default
parent: Concepts
---

The post-quantum algorithms available today are relatively new compared to traditional asymmetric cryptography algorithms like elliptic curves. Because of this, Sare employs a combination of both traditional and post-quantum algorithms. This approach ensures that, in the event our post-quantum algorithms are compromised, we still maintain protection against non-quantum attacks.

To achieve this, Sare utilizes a post-quantum Key Encapsulation Mechanism (KEM) and a Diffie-Hellman key exchange algorithm to derive two separate shared secrets.

## Concatenation of Shared Secrets

To derive an encryption key from these two shared secrets, Sare concatenates them and passes the result as Initial Keying Material (IKM) to an HKDF with an 8-byte salt. The process extracts a Pseudo Random Key (PRK) as the encryption key. This key is then employed in a Stream AEAD cipher to both encrypt and decrypt files.

```
+----------------------+
|   Post-Quantum KEM   |
|   (Shared Secret 1)  |
+----------------------+
            |
            V
+----------------------+
| Diffie-Hellman Key   |
| Exchange (Shared     |
| Secret 2)             |
+----------------------+
            |
            V
+----------------------------------+
| Concatenation of Shared Secrets  |
| (Shared Secret 1 + Shared Secret 2)|
+----------------------------------+
            |
            V
+----------------------------------+
| HKDF with 8-byte Salt            |
| (Initial Keying Material - IKM)  |
+----------------------------------+
            |
            V
+------------------------+
|   Pseudo Random Key   |
|   (PRK - Encryption Key)|
+------------------------+
            |
            V
+------------------------+
|   Stream AEAD Cipher   |
|   (Encryption/Decryption)|
+------------------------+

```

