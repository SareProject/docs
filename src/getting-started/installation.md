
# Installation

### Installing SARE CLI from Source

You can build SARE CLI directly from source. This is useful if you want the latest development version or need to tweak the code:

```bash
git clone https://github.com/SareProject/sare
cd sare
cargo build --release
```

The resulting binary will be located at:

```bash
target/release/sare-cli
```

You can then move it to a directory in your PATH for easier access:

```bash
mv target/release/sare-cli /usr/local/bin/sare-cli
```

### Installation Using Cargo

For a simpler installation, you can install the latest released version directly from crates.io:

```bash
cargo install sare-cli
```

After installation, the `sare-cli` command will be available globally in your terminal.

---

## Using SARE Lib in a Rust Project

To use SARE as a library in your Rust projects, add it to your dependencies in `Cargo.toml`:

```toml
[dependencies]
sare-lib = "0.1"    
```

### Using Cargo to Add the Dependency

Alternatively, you can add `sare-lib` directly using Cargo:

```bash
cargo add sare-lib
```

This will automatically update your `Cargo.toml` and fetch the library from crates.io, making it ready for use in your project.
