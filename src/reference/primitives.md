# Cryptographic Primitives

Cryptographic primitives are the **building blocks** of secure systems.  
They include encryption algorithms, signature schemes, key exchange methods, and key derivation functions.  
SARE combines **classical elliptic-curve cryptography** with **post-quantum algorithms** to provide long-term security.

---

## Symmetric Cryptography

Symmetric algorithms are used for encrypting file contents and wrapping keys.  
They are efficient and secure, even in the post-quantum era.

- **XChaCha20-Poly1305** — AEAD algorithm for streaming encryption with confidentiality and integrity.  
- **AES-256-GCM** — AEAD alternative based on AES.  
- **AES-256 Key Wrap (AES-KW)** — Used to wrap (encrypt) symmetric keys securely.  

Each encrypted file includes a unique **nonce** in its metadata to prevent key/nonce reuse attacks.

---

## Asymmetric Cryptography

Asymmetric primitives are used for exchanging keys, verifying identities, and creating digital signatures.

- **Classical algorithms**:  
  - `Ed25519` (signatures)  
  - `X25519` (key exchange)

- **Post-quantum algorithms**:  
  - `Dilithium3` (signatures)  
  - `Kyber` (key encapsulation mechanism)

---

## Hybrid KEM

SARE’s **Hybrid KEM** combines:  
- A classical Diffie-Hellman exchange (`X25519`)  
- A post-quantum KEM (`Kyber`)  

This produces two shared secrets.  
SARE then uses **HKDF** to derive a final symmetric key by hashing the concatenation of both secrets.  
This ensures security against both classical and quantum adversaries.  
[Read more →](./hybrid-kem.md)

---

## Hybrid Signatures

When signing data, SARE generates **two signatures**:  
- One classical (`Ed25519`)  
- One post-quantum (`Dilithium3`)  

Both signatures must verify successfully.  
This ensures trust even if one algorithm is broken in the future.  
[Read more →](./hybrid-signatures.md)

---

## Key Derivation Functions (KDFs)

Key derivation is central to SARE:  

- **HKDF**: Derives symmetric keys from shared secrets in Hybrid KEM.  
- **PKDF**: Derives keys from user passphrases when encrypting with a password.  

Each derived key is tied to a salt and algorithm identifier, preventing reuse across contexts.  
[Read more →](./kdfs.md)

---

## Randomness & Nonces

Every encryption uses secure randomness:  
- Nonces ensure ciphertext uniqueness.  
- Salts in KDFs prevent rainbow-table attacks.  

The randomness source is the system’s **cryptographically secure RNG**.

---

## Summary

| Primitive Type      | Algorithm(s) Used       | Purpose                                   |
| ------------------- | ----------------------- | ----------------------------------------- |
| Symmetric           | XChaCha20-Poly1305, AES | File encryption, AEAD                     |
| Key Wrapping        | AES-KW                  | Encrypting symmetric keys                 |
| Asymmetric (class.) | X25519, Ed25519         | DH exchange, digital signatures           |
| Asymmetric (PQ)     | Kyber, Dilithium3       | PQ KEM, PQ signatures                     |
| KDFs                | HKDF, PKDF              | Deriving keys from secrets or passphrases |

These primitives form the secure foundation for SARE’s encryption, signatures, and certificate model.
