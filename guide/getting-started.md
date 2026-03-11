# Getting Started

Pick your language below for a quick first round-trip.

## Rust

```toml
[dependencies]
ason = "0.1"
serde = { version = "1", features = ["derive"] }
```

```rust
use ason::{decode, encode_typed, Result};
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

    let text = encode_typed(&users)?;
    let restored: Vec<User> = decode(&text)?;
    assert_eq!(users, restored);
    Ok(())
}
```

See the full [Rust guide](/languages/rust).

## Go

```bash
go get github.com/ason-lab/ason-go
```

```go
package main

import (
    "fmt"
    ason "github.com/ason-lab/ason-go"
)

type User struct {
    ID     int64  `ason:"id"`
    Name   string `ason:"name"`
    Active bool   `ason:"active"`
}

func main() {
    users := []User{
        {ID: 1, Name: "Alice", Active: true},
        {ID: 2, Name: "Bob", Active: false},
    }

    text, _ := ason.EncodeTyped(users)
    var out []User
    _ = ason.Decode(text, &out)
    fmt.Println(out)
}
```

See the full [Go guide](/languages/go).

## Python

```bash
cd ason-py
python3 -m pip install -e .
```

```python
import ason

users = [
    {"id": 1, "name": "Alice", "active": True},
    {"id": 2, "name": "Bob", "active": False},
]

text = ason.encodeTyped(users)
restored = ason.decode(text)

assert restored == users
```

See the full [Python guide](/languages/python).

## Other Languages

| Language | Guide |
|----------|-------|
| C | [C guide](/languages/c) |
| C++ | [C++ guide](/languages/cpp) |
| Java | [Java guide](/languages/java) |
| Zig | [Zig guide](/languages/zig) |

## Tooling

- Read the [spec](/spec) for the canonical format definition.
- Use the VS Code extension for syntax highlighting and preview.
- Use the compatibility and benchmark pages when comparing implementations.
