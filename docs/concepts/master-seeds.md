---
title: Master Seeds
layout: default
parent: Concepts
---

Sare is a hybrid encryption library/standard/tool that utilizes both post-quantum algorithms and traditional algorithms. This means we would have to manage multiple keys, some of which may be quite long, potentially leading to complications later and making backups challenging.

Here's where Sare's master seeds come into play—a master seed from which every other key will be derived. It's a master seed short enough to write on a piece of paper yet long enough to be practically impossible to guess.

## How Master Seeds Are Generated

Master seeds are created by generating 128 bytes (1024 bits) of cryptographically secure random entropy. Child seeds or child keys are then derived from this master seed.

But is 128 bytes of data too short? No, it's actually more than enough. To put it in perspective, consider the AES256 key size, which is 32 bytes and is considered practically impossible to guess through brute force. To illustrate, here are the possibilities for both:

- Possibilities to guess AES256's key: 115,792,089,237,316,195,423,570,985,008,687,907,853,269,984,665,640,564,039,457,584,007,913,129,639,936
- Possibilities to guess Sare's master seed: 179,769,313,486,231,590,772,930,519,078,902,473,361,797,697,894,230,657,273,430,081,157,732,675,805,500,963,132,708,477,322,407,536,021,120,113,879,871,393,357,658,789,768,814,416,622,492,847,430,639,474,124,377,767,893,424,865,485,276,302,219,601,246,094,119,453,082,952,085,005,768,838,150,682,342,462,881,473,913,110,540,827,237,163,350,510,684,586,298,239,947,245,938,479,716,304,835,356,329,624,224,137,216

Still, it can be converted to a mnemonic seed like those used in Bitcoin, consisting of only 96 words. Although long, it remains manageable.

```
+--------------------------------------+
|    Cryptographically Secure RNG     |
|          (1024 Bits / 128 Bytes)     |
+--------------------------------------+
                    |
                    V
    +----------------------------------+
    |         Master Seed              |
    | (Generated from Random Entropy)  |
    +----------------------------------+
                    |
                    V
    +----------------------------------+
    |  Mnemonic Seed (96 Words)        |
    +----------------------------------+

```



## Deriving Child Seeds from Master Seed

Child seed/key derivation is accomplished using HKDFs. For child seeds/keys needing 32 bytes, SHA2/SHA3_256 is used; for those requiring 64 bytes, SHA2/SHA3_512 is employed. Larger child keys are derived from a 64-byte child seed fed into an XOF. The use of different hash functions for varying key sizes ensures not relying on one hash algorithm for everything. In the event one hash algorithm is compromised, others for different key sizes remain secure. When deriving child seeds/keys, the entire master seed is provided to the HKDF as IKM, with the last 10 bytes of the master seed serving as the salt and a 4-byte constant context for each algorithm to ensure the uniqueness of child keys.



## Storing Master Seeds

Master seeds can be stored either encrypted or unencrypted. When encrypted, AES265 KeyWrap mode and a password-based KDF like scrypt are employed to derive the key for encryption from a passphrase.

```
+-----------------------+
|     Master Seed       |
|  (Generated Randomly) |
+-----------------------+
            |
            V
+---------------------+
|      Passphrase     |
|        +            |
|        V            |
| +----------------+  |
| |      KDF       |  |
| |    (Scrypt)    |  |
| +----------------+  |
|        |            |
|        V            |
| +------------------+ |
| |       Key        | |
| |                  | |
| +------------------+ |
|        |            |
|        V            |
| +------------------+ |
| |  AES KeyWrap     | |
| | (Using Master    | |
| |     Seed)        | |
| +------------------+ |
|        |            |
|        V            |
| +------------------+ |
| | Encrypted Master | |
| |      Seed        | |
| +------------------+ |

```

