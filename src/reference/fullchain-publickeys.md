# FullChain Public Keys

A **FullChain Public Key** in SARE combines both **signature** and **encryption** public keys into a single, exportable structure. It is used to verify signatures and encrypt data for the holder of the corresponding master key.

## Structure

The full chain contains two main components:

```rust
#[derive(Serialize, Deserialize, Clone)]
pub struct FullChainPublicKeyFormat {
    pub signature_public_key: SignaturePublicKeyFormat,
    pub encryption_public_key: EncryptionPublicKeyFormat,
}
```

### Signature Public Key (`SignaturePublicKeyFormat`)

- Combines **classical elliptic-curve (EC)** and **post-quantum (PQ)** algorithms:
  

```rust
pub struct SignaturePublicKeyFormat {
    pub ec_algorithm: ECAlgorithm,
    pub pq_algorithm: PQAlgorithm,
    pub ec_public_key: Vec<u8>,
    pub pq_public_key: Vec<u8>,
}
```

- Both EC and PQ signatures must verify successfully for a message to be trusted.
  

### Encryption Public Key (`EncryptionPublicKeyFormat`)

- Combines **Diffie-Hellman (DH)** and **hybrid KEM** algorithms:
  

```rust
pub struct EncryptionPublicKeyFormat {
    pub dh_algorithm: DHAlgorithm,
    pub kem_algorithm: KEMAlgorithm,
    pub dh_public_key: Vec<u8>,
    pub kem_public_key: Vec<u8>,
}
```

- Used for hybrid KEM-based key agreement to encrypt secrets for this key holder.
  

## Export Formats

### BSON

- The full chain can be encoded as BSON:
  

```json
{
  "signature_public_key": {
    "ec_algorithm": "EC_ALGO",
    "pq_algorithm": "PQ_ALGO",
    "ec_public_key": "<bytes>",
    "pq_public_key": "<bytes>"
  },
  "encryption_public_key": {
    "dh_algorithm": "DH_ALGO",
    "kem_algorithm": "KEM_ALGO",
    "dh_public_key": "<bytes>",
    "kem_public_key": "<bytes>"
  }
}
```

### PEM

- Encoded as a standard PEM block:
  

```
-----BEGIN SARE FULLCHAIN PUBLIC KEY-----
<base64-encoded BSON>
-----END SARE FULLCHAIN PUBLIC KEY-----
```

## Fingerprint

The **fingerprint** uniquely identifies a full chain public key and is derived deterministically from all contained public key components.

```rust
impl FullChainPublicKeyFormat {
    pub fn calculate_fingerprint(&self) -> [u8; 32] {
        let mut hasher = Sha256::new();

        // EC signature component
        hasher.update(self.signature_public_key.ec_algorithm.to_string().as_bytes());
        hasher.update(&self.signature_public_key.ec_public_key);

        // PQ signature component
        hasher.update(self.signature_public_key.pq_algorithm.to_string().as_bytes());
        hasher.update(&self.signature_public_key.pq_public_key);

        // DH encryption component
        hasher.update(self.encryption_public_key.dh_algorithm.to_string().as_bytes());
        hasher.update(&self.encryption_public_key.dh_public_key);

        // KEM encryption component
        hasher.update(self.encryption_public_key.kem_algorithm.to_string().as_bytes());
        hasher.update(&self.encryption_public_key.kem_public_key);

        let fingerprint: [u8; 32] = hasher.finalize().into();
        fingerprint
    }
}
```

**Notes:**

- Uses SHA-256 over all algorithm identifiers and public key bytes.
  
- Ensures unique identification of each full chain public key.
  
- Safe to share publicly, as it contains only public information.
