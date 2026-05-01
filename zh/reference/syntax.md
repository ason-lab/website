# 语法参考

ASUN 当前文本语法的完整参考。

## 文档形态

ASUN 文本有四种顶层形态：

- schema 对象：`{schema}:(values)`
- schema 对象数组：`[{schema}]:(values),(values)`
- 普通数组：`[value, value]`
- 裸值：`value`

单个对象：

```asun
{id@int, name@str}:(1, Alice)
```

多行列表：

```asun
[{id@int, name@str}]:
  (1, Alice),
  (2, Bob)
```

`:` 前面是 Schema，后面是一条或多条元组数据。

## Schema

### 字段写法

```asun
{id, name, active}
{id@int, name@str, active@bool}
```

在文本 ASUN 中，`@` 是字段绑定符。基本类型提示是可选的。若出现，当前只允许：

- `int`
- `float`
- `str`
- `bool`

对复杂字段，同一个 `@` 会变成必需的结构绑定：

- `@{...}` 嵌套结构体
- `@[type]` 数组
- `@[{...}]` 对象数组

例如：

```asun
{profile@{id@int, name@str}}
{tags@[str]}
{attrs@[{key@str, value@str}]}
```

`id` 与 `id@int` 在结构布局上等价；但 `profile@{...}`、`tags@[...]` 必须保留 `@`，否则解析器无法识别嵌套结构边界。

### 字段名

简单字段名可以不加引号：

```asun
{id, name, active}
```

裸字段名只能包含 ASCII 字母、数字和 `_`，数字可以出现在开头。

如果字段名：

- 含空格
- 含 `+`、`-`、`.` 或其它标点
- 含 `{ } [ ] @ "` 等语法字符

就必须加引号：

```asun
{65@bool, "id uuid"@int, "a+b"@str, "{}[]@\""@str}
```

## 数据

数据是位置型的，值严格按 Schema 顺序对应。

```asun
{id@int, name@str, active@bool}:(1, Alice, true)
```

嵌套结构体的数据写成嵌套元组：

```asun
{user@{id@int, name@str}}:((1, Alice))
```

当前格式不支持内联对象字面量。

## 标量值

### `int`

```asun
42
-7
0
```

### `float`

```asun
3.14
-0.5
1e10
1.5e-3
-1.0E+100
```

浮点数要求小数点前必须有数字；如果写了小数点，小数点后也必须至少有一位数字。指数使用 `e` 或 `E`，可带 `+` / `-` 符号。

这些 token 是字符串，不是数字：`.5`、`5.`、`+5`、`1e`、`1e+`。

### `bool`

```asun
true
false
```

### null / optional

元组或数组里的空槽表示 null / 缺失：

```asun
{id@int, label@str}:(1, )
[1,,3]
```

尾随逗号会被吸收，不会额外增加 null。要表达末尾 null，需要连续逗号：`(a,b,,)`。

## 字符串

### 不带引号字符串

简单值可以不加引号：

```asun
Alice
hello world
```

规则：

- 首尾空白会被 trim
- 原始的 `, ( ) [ ] { } : @ " \` 和控制字符不允许出现在不带引号字符串中
- 原始 `/`、`<`、`>` 允许出现，但 `/*` 会开始块注释
- 如需保留语法字符，请使用引号或转义

```asun
{path@str}:(path/to/file)
{name@str}:("@Alice")
```

### 带引号字符串

需要保留空白或包含保留字符时使用引号：

```asun
"value with, comma"
"  leading spaces  "
"line\nbreak"
```

支持的转义包括 `\"`、`\\`、`\n`、`\t`、`\r`、`\b`、`\f`、`\,`、`\(`、`\)`、`\[`、`\]`、`\{`、`\}`、`\:`、`\@` 和 `\uXXXX`。

## 数组

```asun
[{name@str, scores@[int]}]:
  (Alice, [90, 85, 92]),
  (Bob,   [76, 88])
```

也允许嵌套数组：

```asun
[{matrix@[[int]]}]:
  ([[1, 2], [3, 4]])
```

## 条目列表

键值集合写成条目列表：

```asun
[{name@str, attrs@[{key@str, value@int}]}]:
  (Alice, [(age, 30), (score, 95)])
```

## 注释

ASUN 在可出现可选空白的位置支持块注释：

```asun
/* user list */
[{id@int, name@str}]:
  (1, Alice),
  (2, Bob)
```

行注释不是格式的一部分。

注释不能出现在带引号字符串、不带引号字符串、数字、布尔值、元组或数组字面量内部。Conformance 测试会把 `(...)` 与 `[...]` 内部的注释标记为错误。

## 空白规则

- 结构 token 之间的空白会被忽略
- 不带引号字符串会在外层自动 trim
- 推荐使用多行美化布局，但不是必须
