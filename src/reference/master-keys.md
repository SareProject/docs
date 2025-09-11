# Master Keys

A **Master Key** in SARE is the root secret that deterministically derives all cryptographic keys (encryption, signing, hybrid, etc.). Its security relies on the **master seed**, which must be stored securely.

## Master Seed

- The **master seed** is a high-entropy secret, stored as a `SecretVec<u8>`.
  
- Generated via a cryptographically secure RNG.
  
- Used to deterministically derive all hybrid KEM/Sign keypairs.
  

## Master Key Storage Formats

### Secret Key Format (`SecretKeyFormat`)

```rust
#[derive(Serialize, Deserialize)]
pub struct SecretKeyFormat {
    pub ec_algorithm: ECAlgorithm,
    pub pq_algorithm: PQAlgorithm,
    pub dh_algorithm: DHAlgorithm,
    pub kem_algorithm: KEMAlgorithm,
    #[serde(with = "secret_vec_serde")]
    pub master_seed: SecretVec<u8>,
    #[serde(skip_serializing_if = "Option::is_none", flatten)]
    pub encryption_metadata: Option<EncryptionMetadataFormat>,
}
```

- **master_seed**: the root secret (raw or encrypted).
  
- **encryption_metadata**: optional; only present if the seed is encrypted with a password. Contains either:
  
    - `pkdf_metadata` – information about the password-based key derivation function (algorithm, salt)
      
    - `kem_metadata` – for hybrid KEM-based wrapping of the seed (optional).
      

**Key points:**

- If `encryption_metadata.pkdf_metadata` is **present**, SARE will attempt to decrypt the seed using AES Key Wrap with the key derived via the PKDF.
  
- If `encryption_metadata.pkdf_metadata` is **absent**, the seed is **raw** and used directly.
  

### BSON Representation

- Stored as a standard BSON document:
  

```json
{
  "ec_algorithm": "EC_ALGO_NAME",
  "pq_algorithm": "PQ_ALGO_NAME",
  "dh_algorithm": "DH_ALGO_NAME",
  "kem_algorithm": "KEM_ALGO_NAME",
  "master_seed": "<raw or encrypted bytes>",
  "encryption_metadata": {
      "pkdf_metadata": {
          "pkdf_algorithm": "PBKDF2 | Argon2 | ...",
          "pkdf_salt": "<salt bytes>"
      },
      "kem_metadata": { ... } // optional
  }
}
```

- The **`encryption_metadata`** field is optional.
  
- **Raw seeds** omit `encryption_metadata`.
  

### PEM Representation

- The entire BSON (raw or encrypted) is Base64-encoded into PEM:
  

```
-----BEGIN SARE MASTER KEY-----
<base64-encoded BSON or encrypted seed>
-----END SARE MASTER KEY-----
```

- Can be imported/exported via CLI or programmatic interfaces.
  

## Encrypted Master Keys

1. **Password Key Derivation**
   
    - If a password is provided, SARE uses the **PKDF algorithm** specified in `pkdf_metadata` to derive an AES-KW key.
      
    - Example PKDFs: PBKDF2, Argon2.
    
2. **AES Key Wrap**
   
    - The derived key encrypts (wraps) the master seed, producing a protected `master_seed` in `SecretKeyFormat`.
    
3. **Optional KEM Wrapping**
   
    - For hybrid setups, `kem_metadata` can store a KEM-based encrypted seed instead of or in addition to PKDF.
    
4. **Decryption Logic**
   
    - If `encryption_metadata.pkdf_metadata` exists, SARE derives the AES-KW key and unwraps the seed.
      
    - Otherwise, the seed is treated as raw.
      

## Fingerprint Calculation

- The fingerprint is SHA256 over the `master_seed` bytes:
  

```rust
impl SecretKeyFormat {
    pub fn calculate_fingerprint(master_seed: SecretVec<u8>) -> Vec<u8> {
        let mut hasher = Sha256::new();
        hasher.update(master_seed.expose_secret());
        let fingerprint: [u8; 32] = hasher.finalize().into();
        fingerprint[..=16].to_vec()
    }
}
```

- Used to uniquely identify the master key without exposing the seed.
