# Rust 指南

Rust 是目前最成熟的 ASON 实现，也最适合完整了解文本与二进制 API 的形态。

## 安装

```toml
[dependencies]
ason = "0.1"
serde = { version = "1", features = ["derive"] }
```

## 文本 API

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

## 二进制 API

```rust
use ason::{decode_binary, encode_binary, Result};

let bytes = encode_binary(&users)?;
let restored: Vec<User> = decode_binary(&bytes)?;
```

## API 概览

| 函数 | 作用 |
|------|------|
| `encode` / `encode_typed` | 紧凑文本输出 |
| `encode_pretty` / `encode_pretty_typed` | 美化文本输出 |
| `decode` | 解码 typed 或 untyped 文本 |
| `encode_binary` | 二进制编码 |
| `decode_binary` | 二进制解码 |

## 运行示例

```bash
cd ason-rs
cargo run --release --example basic
cargo run --release --example bench
cargo run --release --example complex
cargo run --release --example cross_compat
```

## 运行测试

```bash
cd ason-rs
cargo test
```

## 性能说明

Rust 目前是 ASON 生态里 benchmark 最成熟的一条实现，但不应该把 Rust 的数字直接当成所有语言的统一结论。更细的工作负载说明见 [benchmark notes](/zh/reference/benchmark-notes)。
