# Technical Reference

This section provides a deep dive into the internal structure and cryptographic foundations of **SARE**.  
It is intended for developers and security researchers who want to understand how SARE works under the hood, integrate it into their own applications, or audit its design.  

SARE is split into three layers:

- **sare-core**: Low-level cryptographic operations and file format handling.
- **sare-lib**: High-level library built on top of `sare-core` to make integration easy.
- **sare-cli**: A command-line interface that demonstrates and exposes SARE’s features.

## Contents

- [SARE File Format](./file-format.md)  
  Describes how encrypted files are structured, including metadata, nonces, checksums, and signatures.

- [Cryptographic Primitives](./primitives.md)  
  The algorithms and operations that form the foundation of SARE, including hybrid KEM, hybrid signatures, and KDFs.

- [Certificates](./certificates.md)  
  Validation and revocation certificates, their structure, and their role in the trust model.

- [SareDB](./saredb.md)  
  A CLI-only JSON database for managing keys and recipients. Not part of the library but used in `sare-cli`.

## Design Notes

- **Self-describing encryption**: Metadata contains everything needed to decrypt.  
- **Hybrid classical + post-quantum**: Ensures security both today and in a future with large-scale quantum computers.  
- **Clear separation of layers**: `sare-core` provides primitives, `sare-lib` simplifies usage, and `sare-cli` offers practical workflows.  

For detailed information, follow the links in each section.
