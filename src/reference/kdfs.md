# Key Derivation Functions (KDFs) in SARE

In SARE, cryptographic keys are derived systematically to ensure security and uniqueness across different algorithms and use cases. Two main categories of KDFs are employed: **HKDF** for general key derivation and **password-based KDFs (PKDFs)** for deriving keys from user-supplied secrets like passwords.

## HKDF-Based Key Derivation

SARE uses the HMAC-based Key Derivation Function (**HKDF**) to expand secret material into cryptographic keys. The specific hash function used depends on the desired key size:

* **32-byte keys (256-bit):** Use **SHA-256**.
* **64-byte keys (512-bit):** Use **SHA-512**.

Each HKDF derivation includes **algorithm-specific magic bytes** to ensure that even if the same input material is used across multiple algorithms, the resulting keys are unique. These magic bytes are prepended or mixed into the input seed when deriving keys for different algorithms.

**Example usage:**

```text
Key size: 32 bytes → SHA-256 HKDF
Magic bytes: [25, 85, 210, 14] (for Ed25519, for instance)

Key size: 64 bytes → SHA-512 HKDF
Magic bytes: [104, 7, 0, 0] (for Kyber768, for instance)
```

This mechanism guarantees that each derived key is tied not only to the input seed but also to the cryptographic algorithm and context, preventing key collisions.

## Password-Based Key Derivation (PKDF)

For symmetric encryption or key wrapping, SARE derives keys from passwords using **scrypt**. This function applies configurable parameters to increase computational cost and memory usage, defending against brute-force attacks.

**Typical flow:**

1. Generate a random salt (8 bytes by default).
2. Apply scrypt with specified parameters `(N, r, p)` to the password and salt.
3. Extract a key of the required length.

Scrypt ensures that even weak passwords produce keys resistant to offline attacks, and the salt guarantees that identical passwords produce different keys.

**Example parameters:**

```text
N = 2^5 = 32
r = 8
p = 1
```

Derived keys from PKDFs can then be safely used for:

* Symmetric encryption
* Key wrapping for secure storage
* Hybrid cryptographic constructions alongside HKDF-derived keys

---

### Summary

* **HKDF**: Produces algorithm-specific keys using magic bytes, keyed on SHA-256 or SHA-512 depending on the output size.
* **PKDF (scrypt)**: Converts passwords into cryptographic keys, incorporating salt and computationally intensive parameters to protect against brute-force attacks.
* All derived keys are unique per algorithm and purpose due to the combination of input material, salt, and magic bytes.
