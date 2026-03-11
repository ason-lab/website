# Go Guide

The Go implementation provides reflection-based encoding and decoding for ASON text and binary formats.

## Install

```bash
go get github.com/ason-lab/ason-go
```

## Text API

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

Use `EncodeTyped` when you need type-preserving round-trips. Use `Encode` when you want shorter untyped text.

## Binary API

```go
data, _ := ason.EncodeBinary(user)

var out User
_ = ason.DecodeBinary(data, &out)
```

## Field Tags

Use `ason:"fieldname"` tags to control schema field names.

```go
type Product struct {
    ProductID   int64   `ason:"id"`
    ProductName string  `ason:"name"`
    Price       float64 `ason:"price"`
}
```

## Run Examples

```bash
cd ason-go
go run ./examples/basic
go run ./examples/bench
go run ./examples/complex
```

## Run Tests

```bash
cd ason-go
go test ./...
```

## Performance Notes

Go usually benefits most on repeated structs and arrays. For implementation-specific notes, see [benchmark notes](/reference/benchmark-notes).
