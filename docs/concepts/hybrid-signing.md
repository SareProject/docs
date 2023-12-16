---
title: Hybrid Signing
layout: default
parent: Concepts
---

In Sare, signatures utilize two separate keypairs, similar to the Hybrid Key Encapsulation Mechanism (KEM), and for the same reason—comprising one elliptic curve signing keypair and one post-quantum signing keypair.

## How the Data Is Signed

Data signing is achieved by independently signing it with each of the keypairs. For verification, the message is then verified separately with each signature. If either of the verifications fails, the signature is considered invalid. If both the post-quantum and elliptic curve signatures verify successfully, the data is deemed valid.

```
+----------------------+
| Elliptic Curve       |
| Signing Keypair      |
+----------------------+
            |
            V
+------------------------+
|    Data Signing with   |
|    Elliptic Curve Key  |
+------------------------+
            |
            V
+----------------------+
| Post-Quantum         |
| Signing Keypair      |
+----------------------+
            |
            V
+------------------------+
|    Data Signing with   |
|    Post-Quantum Key    |
+------------------------+
            |
            V
+----------------------------------+
|       Separate Storage of         |
|  Elliptic Curve Signature and     |
|  Post-Quantum Signature           |
+----------------------------------+
            |
            V
+----------------------------------+
|       Data Verification          |
|    (Separate Verification with   |
|     Each Signature)              |
+----------------------------------+
            |
            V
+----------------------------------+
|        Valid/Invalid Decision     |
|   (Both Signatures Must Verify   |
|   for Data to be Considered Valid)|
+----------------------------------+


```

