# Hybrid Signatures

**Hybrid Signatures** in SARE combine a **classical digital signature** with a **post-quantum signature** to ensure long-term integrity and non-repudiation of files.

The rationale for using a hybrid approach is to **future-proof signatures** against quantum attacks while maintaining compatibility with current cryptographic standards. By combining classical and post-quantum signatures, SARE ensures that a file remains verifiable even if one algorithm is later broken.

## How Hybrid Signatures Work

When a file is signed in SARE:

1. **Classical Signature Generation**
   
    - Typically uses `Ed25519`.
      
    - Signs the **SHA3-256 checksum** of the file or message.
    
2. **Post-Quantum Signature Generation**
   
    - Uses a PQ signature algorithm (e.g., `Dilithium` or `Falcon`, depending on configuration).
      
    - Also signs the **same file checksum**.
      

Both signatures are **generated independently** but stored together.

**Verification Rule:**

- A file is only considered **authentic** if **both signatures verify correctly**.
  
- Failure of either signature invalidates the file’s integrity.
  

## Signature Formatting

SARE stores signatures in a structured format included in the file header:

```text
SignatureFormat {
    ec_signature: Vec<u8>,  // e.g., Ed25519
    pq_signature: Vec<u8>,         // e.g., Dilithium
    fullchain_fingerprint: [u8; 32], // Identifier for the keychain used
    signature_metadata: Option<...>,  // Additional metadata such as version, key ID
}
```

- `ec_signature` – The bytes of the classical signature.
  
- `pq_signature` – The bytes of the post-quantum signature.
  
- `fullchain_fingerprint` – Fingerprint of the signer’s keychain to identify which key generated the signature.
  
- `signature_metadata` – Optional metadata that may include algorithm versioning or key identifiers.
  

**End Result:**

- When a file is encrypted or signed, the hybrid signature is attached to the file header.
  
- Tools like the CLI will verify both signatures and report validity.
