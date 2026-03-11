# Zig Guide

The Zig implementation uses comptime metadata to generate schema-aware text and binary codecs.

## Installation

Add to `build.zig.zon`:

```zig
.dependencies = .{
    .ason = .{
        .url = "https://github.com/ason-lab/ason-zig/archive/refs/heads/main.tar.gz",
    },
},
```

Then in `build.zig`:

```zig
const ason = b.dependency("ason", .{ .target = target, .optimize = optimize });
exe.root_module.addImport("ason", ason.module("ason"));
```

## Usage

```zig
const std = @import("std");
const ason = @import("ason");

const User = struct {
    id: i64,
    name: []const u8,
    active: bool,
};

pub fn main() !void {
    var gpa = std.heap.GeneralPurposeAllocator(.{}){};
    defer _ = gpa.deinit();
    const allocator = gpa.allocator();

    const users = [_]User{
        .{ .id = 1, .name = "Alice", .active = true },
        .{ .id = 2, .name = "Bob", .active = false },
    };

    const text = try ason.encodeVec(User, &users, allocator);
    defer allocator.free(text);

    const typed = try ason.encodeVecTyped(User, &users, allocator);
    defer allocator.free(typed);

    const restored = try ason.decodeVec(User, typed, allocator);
    defer allocator.free(restored);

    const bytes = try ason.encodeBinaryVec(User, &users, allocator);
    defer allocator.free(bytes);

    const restored2 = try ason.decodeBinaryVec(User, bytes, allocator);
    defer allocator.free(restored2);
}
```

## Comptime-Friendly API

```zig
const typed = try ason.encodeTyped(User, .{
    .id = 1,
    .name = "Alice",
    .active = true,
}, allocator);
```

## Building Examples

```bash
cd ason-zig
zig build run-basic
zig build run-bench
zig build run-complex
```

## Running Tests

```bash
cd ason-zig && zig build test
```
