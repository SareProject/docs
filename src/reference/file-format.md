# File Formatting in SARE

SARE defines its own structured formats for both **encrypted files** and **signature files**, providing consistency, versioning, and support for cryptographic metadata. This ensures that files can be properly decoded, verified, and decrypted across different SARE implementations.

## Encrypted Files Format

Encrypted files in SARE are not just raw ciphertext—they include a **structured header** containing all metadata necessary for decryption and verification.

### Structure

1. **Magic bytes**:  
    A predefined sequence to identify the file as a SARE-encrypted message.
    
2. **Version**:  
    Each file includes a version number to allow backward and forward compatibility. The current SARE version is embedded in the header.
    
3. **Encryption metadata**:  
    This metadata is part of the file header and includes:
    
    - The symmetric encryption algorithm used (e.g., XChaCha20-Poly1305 for files).
      
    - Nonces for stream encryption.
      
    - Optional PKDF metadata if password-based encryption is used.
      
    - Optional KEM metadata if asymmetric encryption is used, describing the key encapsulation and shared secrets.
      

### Encoding and Decoding

- The **header** is encoded in **binary**.
  
- The metadata is **BSON-serialized**, allowing easy extension while keeping the structure machine-readable.
  
- During decryption, SARE reads the header, extracts metadata, derives the encryption key (via PKDF or HybridKEM + HKDF), and then decrypts the file content.
  

> Note: Encrypted files are strictly binary-encoded. PEM/ASCII-armoring is not used for encrypted files.

## Signature Files Format

Signature files in SARE store cryptographic signatures independently of encrypted content. They follow a structured, self-contained format.

### Structure

1. **Magic bytes**:  
    Each signature file starts with `SARESIGN` to identify it as a SARE signature.
    
2. **Versioning**:  
    A `u32` version number is included to support backward and forward compatibility.
    
3. **Signature metadata**:  
    Metadata includes:
    
    - EC and post-quantum (PQ) algorithms used.
      
    - Optional cryptographic metadata, such as public keys and algorithm parameters.
    
4. **Signature content**:
   
    - Public keys (EC and PQ)
      
    - The message itself (optional; `None` for detached signatures)
      
    - EC and PQ signature values
      
    - Full-chain fingerprint for verification
      

### Encoding and Decoding

- Signature files can be encoded in **binary** (via BSON) or as **PEM**-encoded files:
  
    - Binary encoding is standard.
      
    - PEM encoding provides ASCII-armored files, though SARE CLI currently does not implement the armor option.
    
- Methods:
  
    - `encode_with_magic_byte()` / `decode_with_magic_byte()` handle the header and magic bytes.
      
    - `encode_bson()` / `decode_bson()` handle metadata serialization.
      
    - `encode_pem()` / `decode_pem()` allow ASCII-armored serialization.
      

### Notes

- Certificates in SARE use the same **BSON-encoded signature format** but are documented separately.
  
- Detached and attached signatures are supported; the presence of `message` indicates an attached signature.
  

---

This design ensures that both **encrypted files** and **signature files** are self-describing, versioned, and extensible while remaining secure and compatible across SARE implementations.
