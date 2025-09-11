# Hybrid KEM (Key Encapsulation Mechanism)

SARE uses a **Hybrid Key Encapsulation Mechanism (KEM)** to securely derive encryption keys for asymmetric file encryption. The hybrid approach combines a **classical Diffie-Hellman key exchange** with a **post-quantum KEM**, ensuring robust security even against future quantum attacks.

The hybrid KEM generates **two independent shared secrets** that are later combined and processed to produce a single symmetric encryption key for AEAD encryption.

## Supported Algorithms

Currently, SARE supports the following hybrid KEM algorithms:

- **Classical DH:** `X25519`
  
- **Post-Quantum KEM:** `Kyber768`
  

> Each hybrid KEM consists of a classical DH component and a post-quantum KEM component. Additional algorithms can be added as needed.

## Shared Secret Generation

1. **Classical DH Shared Secret**
   
    - Generated using the sender’s DH keypair and the recipient’s DH public key.
      
    - In sare-core: `DiffieHellman::calculate_shared_key() → ss1`.
    
2. **Post-Quantum KEM Shared Secret**
   
    - Generated using the sender’s KEM keypair and the recipient’s KEM public key.
      
    - In sare-core: `Decapsulation::decapsulate() → ss2`.
      

These two secrets are **independent** and form the basis of the final symmetric key derivation.

## Encryption Key Derivation with HKDF

SARE combines the two shared secrets as follows in sare-lib:

```text
combined_ss = ss1 || ss2
```

Then it derives the final encryption key using **HKDF-SHA256**:

```text
encryption_key = HKDF(combined_ss, salt, info)
```

- `combined_ss` – Concatenation of the two shared secrets.
  
- `salt` – Randomly generated per encryption session.
  
- `info` – Optional contextual info (can be `None`).
  
- `encryption_key` – Final key used for AEAD encryption.
  

---

This design ensures that **the compromise of one component does not expose the encryption key**, providing post-quantum resilience for encrypted files.
