
# Quick Start with SARE CLI

SARE CLI is a command-line tool to manage keys, encrypt/decrypt files, and sign/verify files using hybrid post-quantum + classical cryptography.

> Note: The CLI is fully functional, but some options (KDF customization, revocation broadcast) are not yet implemented. Defaults are used for now.

---

## 1. Generate a Master Key

```bash
sare-cli masterkey generate
```

This generates a new master key and creates:

- **Validation Certificate:** Confirms your public keys are valid for a specified period.
- **Revocation Certificate:** Can be published if your key is compromised.

> Optional flags such as `--hybrid-kem-algorithm` or `--hybrid-sign-algorithm` are present in the CLI but not fully functional yet.

List available master keys:

```bash
sare-cli masterkey list
```

Remove a master key:

```bash
sare-cli masterkey remove <masterkey_id>
```

Export info about a master key:

```bash
sare-cli masterkey export <masterkey_id>
```

------

## 2. Add or Manage Recipients

Add a recipient (so you can encrypt files for them):

```bash
sare-cli recipient add <path-to-public-key.pem>
```

Remove a recipient:

```bash
sare-cli recipient remove <recipient_id>
```

List all recipients:

```bash
sare-cli recipient list
```

------

## 3. Encrypt and Decrypt Files

### Asymmetric Encryption

Encrypt a file for a recipient:

```bash
sare-cli encrypt asymmetric <input_file> <output_file> --recipient <recipient_id>
```

Decrypt a file:

```bash
sare-cli decrypt <input_file> <output_file> --masterkey-id <masterkey_id>
```

> Note: Currently, KDF and algorithm options are fixed and cannot be modified.

### Symmetric Encryption

Encrypt a file symmetrically:

```bash
sare-cli encrypt symmetric <input_file> <output_file>
```

Decrypt:

```bash
sare-cli decrypt <input_file> <output_file>
```

------

## 4. Sign and Verify Files

Generate a signature for a file:

```bash
sare-cli signature generate <input_file> <sign_file> --masterkey-id <masterkey_id>
```

Verify a signature:

```bash
sare-cli signature verify <sign_file> <input_file>
```

------

## 5. Revocation Certificates

Generate a new revocation certificate (replaces old):

```bash
sare-cli revocation new --reason compromised --masterkey-id <masterkey_id>
```

List revocation certificates:

```bash
sare-cli revocation list
```

> Note: `revocation broadcast` is **not implemented** yet because keyserver support is pending.