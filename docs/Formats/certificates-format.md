---
title: Certificates Format
layout: default
parent: Formats
---

## Certificates

Certificates in SARE are crucial for cryptographic operations, providing authenticity and integrity to key management processes. This document outlines the structure and components of SARE certificates.

### CertificateFormat

- `issuer`: Identifies the entity that issued the certificate.
- `expiry_date`: Indicates the date when the certificate expires. *(Optional)*
- `certificate_type`: Specifies the type of certificate. 

### Types of certificate:
- [RevocationCertificateFormat](./revocation-certificate-format)

[SignatureFormat Documentation](./signature-format)
