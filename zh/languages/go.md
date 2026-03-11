# Go 指南

Go 实现通过反射提供 ASON 文本与二进制格式的编解码能力。

## 安装

```bash
go get github.com/ason-lab/ason-go
```

## 文本 API

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
    user := User{ID: 1, Name: "Alice", Active: true}

    text, _ := ason.Encode(user)
    typed, _ := ason.EncodeTyped(&user)
    pretty, _ := ason.EncodePrettyTyped([]User{user})

    fmt.Println(text)
    fmt.Println(typed)
    fmt.Println(pretty)

    var out User
    _ = ason.Decode(typed, &out)
}
```

需要保真 round-trip 时使用 `EncodeTyped`。如果只是追求更短的文本输出，可以使用 `Encode`。

## 二进制 API

```go
data, _ := ason.EncodeBinary(user)

var out User
_ = ason.DecodeBinary(data, &out)
```

## 字段标签

使用 `ason:"fieldname"` 控制 schema 中的字段名。

```go
type Product struct {
    ProductID   int64   `ason:"id"`
    ProductName string  `ason:"name"`
    Price       float64 `ason:"price"`
}
```

## 运行示例

```bash
cd ason-go
go run ./examples/basic
go run ./examples/bench
go run ./examples/complex
```

## 运行测试

```bash
cd ason-go
go test ./...
```

## 性能说明

Go 在重复结构体和数组场景下通常收益较明显。实现级 benchmark 说明见 [benchmark notes](/zh/reference/benchmark-notes)。
