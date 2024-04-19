---
title: Public Keys
layout: default
parent: Formats
---

## Public Keys

Public keys are crucial components of encryption systems, enabling secure communication and data exchange. This document provides an overview of public keys, including their structure and encoding formats.

### What are Public Keys?

Public keys are cryptographic keys used in asymmetric encryption systems. They are designed to be shared openly and are paired with private keys, which are kept secret. Public keys are used for encryption, digital signatures, and key exchange.

### Types of Public Keys

There are different types of public keys used for various cryptographic purposes:

#### 1. Signature Public Key

- `ec_algorithm`: Specifies the elliptic curve algorithm used for digital signatures.
- `pq_algorithm`: Indicates the post-quantum algorithm used for digital signatures.
- `ec_public_key`: The public key component for the elliptic curve algorithm.
- `pq_public_key`: The public key component for the post-quantum algorithm.

#### 2. Encryption Public Key

- `dh_algorithm`: Specifies the Diffie-Hellman algorithm used for key exchange.
- `kem_algorithm`: Indicates the Key Encapsulation Mechanism (KEM) algorithm used for key encapsulation.
- `dh_public_key`: The public key component for the Diffie-Hellman algorithm.
- `kem_public_key`: The public key component for the Key Encapsulation Mechanism.

### Structure of Public Keys

#### Signature Public Key Format

```
+-----------------+
| ec_algorithm    | Specifies the elliptic curve algorithm used for digital signatures.
| pq_algorithm    | Indicates the post-quantum algorithm used for digital signatures.
| ec_public_key   | The public key component for the elliptic curve algorithm.
| pq_public_key   | The public key component for the post-quantum algorithm.
+-----------------+
```

#### Encryption Public Key Format

```
+-----------------+
| dh_algorithm    | Specifies the Diffie-Hellman algorithm used for key exchange.
| kem_algorithm   | Indicates the Key Encapsulation Mechanism (KEM) algorithm used for key encapsulation.
| dh_public_key   | The public key component for the Diffie-Hellman algorithm.
| kem_public_key  | The public key component for the Key Encapsulation Mechanism.
+-----------------+
```

### Encoding Formats

Public keys can be encoded in different formats for transmission and storage:

- **BSON Data:** Public keys are first BSON-encoded for efficient storage and transmission.
- **PEM Encoding:** BSON-encoded public keys can be further encoded in PEM (Privacy Enhanced Mail) format, which is a common encoding method for cryptographic keys.
