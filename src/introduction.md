# Inroduction

## What is SARE?

<img src="./sare.svg" alt="SARE's Logo" width="300" align="right" style="margin-right:10px;">

SARE (Safe At Rest Encryption) is a **hybrid post-quantum encryption system** designed to protect data against both **classical** and **quantum** attacks.

Unlike traditional encryption tools that rely on a single algorithm (e.g., RSA or AES), SARE combines **classical algorithms** like X25519 and Ed25519 with **post-quantum algorithms** like Kyber768, using a **hybrid scheme**. Even if one algorithm is broken in the future, your data remains secure.


## Why SARE Matters

Quantum computers threaten the foundations of classical cryptography. Algorithms like RSA and ECC will be breakable once sufficiently powerful quantum machines exist.

Post-quantum algorithms are young and may still have vulnerabilities. SARE adopts a **belt-and-suspenders approach**: both classical and post-quantum schemes are used in parallel, giving **robust protection today** and **forward-security tomorrow**.

## Key Features

- **Hybrid Key Encapsulation (KEM):** Diffie–Hellman + PQ KEM to derive shared secrets.
- **Hybrid Signatures:** Files are signed with both elliptic curves and PQ schemes; both signatures must validate.
- **File-Level Encryption:** Files include headers, metadata, ciphertext, and optional signatures.
- **Master Seed:** Single 128-byte seed derives all private keys.
- **Validation Certificates:** Prove that your public keys are valid.
- **Revocation Certificates:** Revoke compromised or obsolete keys.
- **Modular & Extensible:** Swap or add encryption, signing, or KDF algorithms without rewriting core logic.
- **High-Level API via `sare-lib`:** Makes it easy to integrate SARE into Rust applications.
- **CLI (`sare-cli`):** Command-line interface for encryption, decryption, signing, and key management.
- **Security-Focused:** Uses well-vetted, audited algorithms.

## Project Layout

The SARE repository has three main components:

```text
sare-core  - low-level cryptography primitives
sare-lib   - high-level library for file encryption and handling
sare-cli   - command-line interface for everyday use
```

- `sare-core` is low-level and intended to be used through `sare-lib`. Developers should exercise caution if working directly with it.
- `sare-lib` provides the main library API and handles file formatting, encryption, and key management.
- `sare-cli` provides CLI commands for everyday tasks: encrypt, decrypt, sign, manage keys and recipients.

## Installation and Usage

SARE can be used as a CLI tool or integrated into your Rust projects via `sare-lib`.

To get started and install / use SARE please read the [Getting Started](getting-started.md) Page

## Who is SARE For?

- **End users:** Encrypt and sign files with long-term security.
- **Developers:** Integrate hybrid PQ crypto into Rust applications.
- **Researchers:** Experiment with hybrid cryptography in real-world settings.

## Roadmap

- Parallelized encryption/decryption for large files.
- Additional post-quantum algorithms and signature schemes.
- Key revocation workflows in the CLI.
- Keyserver implementations for sharing keys.
- Security audits and performance benchmarks.

## License

- Code: MIT + Apache 2.0
- Books, guides, and documentation: CC BY 4.0
