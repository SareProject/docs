## Example Usage of SARE Lib

### Encrypting and Decrypting Files

SARE Lib allows you to encrypt and decrypt files using either **symmetric** (passphrase-based) or **asymmetric** (public/private key-based) methods. Below are examples for both approaches.

---

#### Symmetric Encryption

Use symmetric encryption when you want to protect a file using just a passphrase. This is fast and simple for personal use or sharing with a small group.

```rust
use sare_lib::{encryption::Encryptor, keys::{EncryptionAlgorithm, RECOMMENDED_PKDF_PARAMS}};
use secrecy::SecretVec;
use std::fs::File;

let input_file = File::open("message.txt")?;
let mut output_file = File::create("message.enc")?;
let passphrase = SecretVec::new(b"supersecret".to_vec());

// Generate key derivation function (KDF) using recommended parameters
let pkdf = Encryptor::get_pkdf(&passphrase, RECOMMENDED_PKDF_PARAMS, 1);

// Encrypt the file symmetrically using XChaCha20-Poly1305 AEAD
Encryptor::encrypt_with_passphrase(&mut input_file, &mut output_file, pkdf, EncryptionAlgorithm::XCHACHA20POLY1305)?;
```

---

#### Symmetric Decryption

To access your encrypted data, provide the same passphrase used during encryption.

```rust
use sare_lib::encryption::Decryptor;
use secrecy::SecretVec;
use std::fs::File;

let mut input_file = File::open("message.enc")?;
let mut output_file = File::create("message_decrypted.txt")?;
let passphrase = SecretVec::new(b"supersecret".to_vec());

Decryptor::decrypt_with_passphrase(passphrase, &mut input_file, &mut output_file)?;
```

---

#### Asymmetric Encryption

For secure sharing with others, use asymmetric encryption. You encrypt with the recipient’s public key, and only they can decrypt with their private key.

```rust
use sare_lib::{encryption::Encryptor, keys::{MasterKey, SharedPublicKey, EncryptionAlgorithm}};
use std::fs::File;

let input_file = File::open("document.txt")?;
let mut output_file = File::create("document.enc")?;

// Load master key and recipient public key
let master_key = MasterKey::load("MASTER_KEY.pem")?;
let recipient_key = SharedPublicKey::from_pem(std::fs::read_to_string("recipient.pem")?)?;

let encryptor = Encryptor::new(master_key);
encryptor.encrypt_with_recipient(&mut input_file, &mut output_file, &recipient_key, EncryptionAlgorithm::XCHACHA20POLY1305)?;
```

---

#### Asymmetric Decryption

The recipient can decrypt the file using their master key. SARE Lib also allows you to detect and verify any attached signatures automatically.

```rust
use sare_lib::encryption::Decryptor;
use std::fs::File;

// Load your master key
let master_key = MasterKey::load("MASTER_KEY.pem")?;
let decryptor = Decryptor::new(master_key);

let mut input_file = File::open("document.enc")?;
let mut output_file = File::create("document_decrypted.txt")?;

// Decrypt the file
let signature = decryptor.decrypt_with_recipient(&mut input_file, &mut output_file)?;

if let Some(sig) = signature {
    println!("Signature attached: {:?}", sig.fullchain_fingerprint);
}
```

---

### Signing and Verifying

SARE allows you to create **detached signatures**, ensuring that your files can be verified without modifying the original content.

#### Detached Signature

```rust
use sare_lib::signing::Signing;
use std::fs;

let master_key = MasterKey::load("MASTER_KEY.pem")?;
let signer = Signing::new(master_key);

let message = fs::read("report.txt")?;
let signature = signer.sign_detached(&message);
fs::write("report.sig", signature.encode_with_magic_byte())?;
```

#### Verifying a Detached Signature

Verification ensures the integrity and authenticity of a file by comparing it with the signature.

```rust
use sare_lib::signing::Signing;
use sare_lib::format::signature::SignatureHeaderFormat;
use std::fs;

let signed_file = fs::read("report.sig")?;
let original_file = fs::read("report.txt")?;

let signature_header = SignatureHeaderFormat::decode_with_magic_byte(&signed_file)?;
let is_valid = Signing::verify_detached(&signature_header, &original_file)?;

println!("Signature valid: {}", is_valid);
```

---

### Master Key Management

Master keys are central to SARE's security. They allow you to manage encryption, signing, and validation certificates.

#### Generate a Master Key

```rust
use sare_lib::keys::{MasterKey, HybridKEMAlgorithm, HybridSignAlgorithm};

let master_key = MasterKey::generate(
    HybridKEMAlgorithm::default(),
    HybridSignAlgorithm::default()
);

// Export the master key (optionally encrypted with a passphrase)
master_key.export(Some("supersecret".as_bytes().to_vec().into()), &mut std::fs::File::create("MASTER_KEY.pem")?)?;

// Export public key
master_key.export_public(&mut std::fs::File::create("PUB_KEY.pem")?)?;
```

#### Inspect a Master Key

You can inspect master keys to verify fingerprints or recover mnemonic seeds.

```rust
let master_key = MasterKey::load("MASTER_KEY.pem")?;
println!("Master Key Fingerprint: {:?}", master_key.get_fullchain_public_fingerprint());
println!("Mnemonic Seed: {}", master_key.to_mnemonic().expose_secret());
```

#### Revoke a Master Key

If a key is compromised or no longer needed, create a revocation certificate to signal that it should not be used.

```rust
use sare_lib::{certificate::Certificate, format::certificate::Issuer, format::certificate::RevocationReason};

let master_key = MasterKey::load("MASTER_KEY.pem")?;
let issuer = Issuer::new("Your Name", "your@email.com");

let revocation_cert = Certificate::new_revocation(master_key, 1682611200, issuer, RevocationReason::NoReasonSpecified);
revocation_cert.export(std::fs::File::create("REVOC_KEY.asc")?)?;
```

---

### Recipient Management

Recipients represent the people you share encrypted files with. SARE makes it easy to add, export, and verify recipient keys.

```rust
use sare_lib::keys::SharedPublicKey;

// Add a recipient
let recipient_key = SharedPublicKey::from_pem(std::fs::read_to_string("recipient.pem")?)?;
recipient_key.export(std::fs::File::create("RECIPIENT.pem")?)?;

// Load and list recipients
let recipient_key = SharedPublicKey::from_pem(std::fs::read_to_string("RECIPIENT.pem")?)?;
println!("Recipient fingerprint: {:?}", recipient_key.fullchain_public_key.calculate_fingerprint());
```