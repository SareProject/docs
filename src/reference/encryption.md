# Encryption and Decryption in SARE

SARE provides both symmetric and asymmetric encryption mechanisms to protect data, combining modern cryptographic primitives with flexible key management.

## Underlying Algorithms

Currently, SARE supports the following primary algorithms:

- **XChaCha20-Poly1305**: Used for symmetric encryption of file contents. Provides strong authenticated encryption with a large nonce space.
  
- **AES-256 Key Wrap (AES-KW)**: Used for securely wrapping smaller cryptographic keys, ensuring confidentiality and integrity of key material.
  

Other algorithms such as AES-GCM are listed but not implemented yet.

## Key Derivation for Encryption

SARE separates the encryption algorithm from the key derivation process, allowing flexibility and additional security measures:

- **Symmetric encryption from passwords**:  
    A passphrase is converted into a cryptographic key using a **password-based key derivation function (PKDF)**. This ensures that even weak passwords are transformed into keys that are safe for encryption.
    
- **Asymmetric encryption for recipients**:  
    Using a **Hybrid Key Encapsulation Mechanism (HybridKEM)**, SARE generates **two shared secrets** between the sender and the recipient. These secrets are combined and expanded via an **HKDF** to produce the final symmetric encryption key.
    
    > Note: Even in asymmetric scenarios, the underlying encryption algorithm remains symmetric (e.g., XChaCha20-Poly1305), providing efficiency and post-quantum security from the key exchange.
    

## Encryption Metadata and File Headers

Encrypted files in SARE are not just raw ciphertext—they include structured metadata to support decryption, verification, and key management:

- **Encryption metadata** includes:
  
    - The algorithm used for encrypting the file (e.g., XChaCha20-Poly1305).
      
    - Nonces for stream encryption.
      
    - Optional key encapsulation metadata (for asymmetric encryption).
      
    - Optional PKDF metadata (for password-based encryption).
    
- **File header**:  
    Every encrypted file contains a header that embeds this encryption metadata along with other metadata, such as digital signatures or comments. This allows SARE to fully describe the encrypted content and recover the key material without requiring external parameters.
    

> The encryption metadata is a defined format within the header but has its own documentation for further details.

## Encrypting and Decrypting Data

- **Symmetric encryption** (e.g., with a passphrase) involves:
  
    1. Deriving a key via PKDF.
       
    2. Initializing an `Encryptor` with the derived key and the chosen algorithm.
       
    3. Writing the structured file header, followed by the encrypted content.
    
- **Asymmetric encryption** (e.g., for a recipient) involves:
  
    1. Generating shared secrets via HybridKEM.
       
    2. Deriving a symmetric encryption key via HKDF.
       
    3. Encrypting the file content using XChaCha20-Poly1305.
       
    4. Embedding key encapsulation and encryption metadata into the file header.
    
- **Decryption** mirrors these processes:
  
    1. Reading and decoding the file header to obtain metadata.
       
    2. Recovering the encryption key (via PKDF or HybridKEM + HKDF).
       
    3. Decrypting the content with the chosen symmetric algorithm.
       

---

This design ensures that SARE encrypted files are **self-describing**, **secure against modern attacks**, and compatible with both password-based and recipient-based key management workflows.
