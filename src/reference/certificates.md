# Certificates in SARE

SARE uses **certificates** to provide verifiable proof about public keys and their status. Certificates in SARE are self-contained, cryptographically signed documents that are **BSON-encoded** and stored in **PEM format** in SARE CLI.

## Types of Certificates

SARE currently supports two main types of certificates:

### 1. Validation Certificates

- These certificates are **attached to a full-chain public key**.
  
- They indicate that a particular public key is valid and can be trusted within a SARE network.
  
- Validation certificates include:
  
    - **Issuer information** (name and email)
      
    - **Expiry date** (optional)
      
    - **Full-chain public key fingerprint** (ensures the certificate is linked to a specific key)
    
- Validation certificates are signed by a SARE master key to ensure authenticity.
  

### 2. Revocation Certificates

- Used to indicate that a previously valid public key or certificate is no longer trustworthy.
  
- Revocation certificates include:
  
    - **Revocation date**
      
    - **Revocation reason** (e.g., `Compromised` or `NoReasonSpecified`)
      
    - **Full-chain fingerprint** of the revoked public key
    
- Revocation certificates are also signed with a SARE master key to prevent tampering.
  

## Certificate Structure

All SARE certificates consist of:

1. **Issuer Metadata**:
   
    - `name`: the name of the issuer
      
    - `email`: the email of the issuer
    
2. **Optional Expiry Date**:
   
    - For validation certificates, defines when the certificate should no longer be considered valid
    
3. **Certificate Type** (via `CertificateType` enum):
   
    - **Validation**: contains `ValidationCertificateFormat` with the full-chain public key fingerprint
      
    - **Revocation**: contains `RevocationCertificateFormat` with revocation details
    
4. **Signature**:
   
    - Every certificate is **cryptographically signed** by the issuing master key.
      
    - SARE uses its internal `SignatureHeaderFormat` to encode the signature alongside the certificate data.
      

## Encoding and Storage

- Certificates are **BSON-encoded**, allowing structured storage of metadata and cryptographic information.
  
- For portability and interoperability, certificates are stored in **PEM format** in SARE CLI. The PEM tag depends on the type:
  
    - Validation certificates: `SARE VALIDATION CERTIFICATE`
      
    - Revocation certificates: `SARE REVOCATION CERTIFICATE`
    
- PEM format provides ASCII-armoring, making the certificates easy to transport, copy, and distribute.
  

## Certificate Operations

SARE provides utilities to handle certificates:

- **Creation**:
  
    - `new_validation()` — create a validation certificate attached to a public key
      
    - `new_revocation()` — create a revocation certificate specifying the revocation reason
    
- **Export**:
  
    - Certificates can be exported as PEM files to any writable stream
    
- **Import**:
  
    - PEM-encoded certificates can be imported and decoded into a SARE certificate object
    
- **Verification**:
  
    - All certificates can be **verified** cryptographically using the attached signature
    
- **Accessing Data**:
  
    - The certificate’s revocation or validation details can be accessed via helper methods
      

---

## Notes

- Both validation and revocation certificates are **signed and verifiable**, ensuring authenticity and integrity.
  
- While SARE CLI uses PEM format for certificates, the underlying representation is **BSON-encoded**, consistent with other SARE formats like encrypted files and signatures.
- Certificates are self-contained and portable, making them suitable for secure key management and revocation in a SARE environment.
