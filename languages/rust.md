# Rust Guide

Rust is the most mature ASON implementation and the best place to see the full text and binary API surface.

## Install

```toml
[dependencies]
ason = "0.1"
serde = { version = "1", features = ["derive"] }
```

## Text API

```rust
use ason::{decode, encode, encode_pretty_typed, encode_typed, Result};
use serde::{Deserialize, Serialize};

#[derive(Debug, Serialize, Deserialize, PartialEq)]
struct User {
    id: i64,
    name: String,
    active: bool,
}

fn main() -> Result<()> {
    let users = vec![
        User { id: 1, name: "Alice".into(), active: true },
        User { id: 2, name: "Bob".into(), active: false },
    ];

    let text = encode(&users)?;
    let typed = encode_typed(&users)?;
    let pretty = encode_pretty_typed(&users)?;

    let restored: Vec<User> = decode(&typed)?;
    assert_eq!(users, restored);

    println!("{text}");
    println!("{pretty}");
    Ok(())
}
```

## Binary API

```rust
use ason::{decode_binary, encode_binary, Result};

let bytes = encode_binary(&users)?;
let restored: Vec<User> = decode_binary(&bytes)?;
```

## API Summary

| Function | Purpose |
|----------|---------|
| `encode` / `encode_typed` | Compact text output |
| `encode_pretty` / `encode_pretty_typed` | Pretty text output |
| `decode` | Decode typed or untyped text |
| `encode_binary` | Binary encoding |
| `decode_binary` | Binary decoding |

## Run Examples

```bash
cd ason-rs
cargo run --release --example basic
cargo run --release --example bench
cargo run --release --example complex
cargo run --release --example cross_compat
```

## Run Tests

```bash
cd ason-rs
cargo test
```

## Performance Notes

Rust currently shows the strongest and most mature benchmark profile in the ASON ecosystem. Use [benchmark notes](/reference/benchmark-notes) for workload-specific context instead of treating Rust numbers as universal for every language.
