# Zig 指南

Zig 实现利用 comptime 元数据生成 schema 驱动的文本与二进制编解码逻辑。

## 安装

在 `build.zig.zon` 中添加依赖：

```zig
.dependencies = .{
    .ason = .{
        .url = "https://github.com/ason-lab/ason-zig/archive/refs/heads/main.tar.gz",
    },
},
```

然后在 `build.zig` 中引入：

```zig
const ason = b.dependency("ason", .{ .target = target, .optimize = optimize });
exe.root_module.addImport("ason", ason.module("ason"));
```

## 使用示例

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

## 适合 comptime 的 API

```zig
const typed = try ason.encodeTyped(User, .{
    .id = 1,
    .name = "Alice",
    .active = true,
}, allocator);
```

## 构建示例

```bash
cd ason-zig
zig build run-basic
zig build run-bench
zig build run-complex
```

## 运行测试

```bash
cd ason-zig && zig build test
```
