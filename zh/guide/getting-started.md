# 快速开始

下面按语言给出一轮最短上手示例。

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

查看完整 [Rust 指南](/zh/languages/rust)。

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

查看完整 [Go 指南](/zh/languages/go)。

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

查看完整 [Python 指南](/zh/languages/python)。

## 其他语言

| 语言 | 指南 |
|------|------|
| C | [C 指南](/zh/languages/c) |
| C++ | [C++ 指南](/zh/languages/cpp) |
| Java | [Java 指南](/zh/languages/java) |
| Zig | [Zig 指南](/zh/languages/zig) |

## 工具

- 规范以 [ASON 格式规范](/zh/spec) 为准。
- VS Code 插件可提供语法高亮与预览。
- 兼容性与 benchmark 页面适合做实现对比。
