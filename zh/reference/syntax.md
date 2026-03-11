# 语法参考

ASON 文本格式的完整参考。

## 文档结构

每个文档都遵循这个形态：

```text
schema ":" data
```

- `schema` 使用 `{...}`
- `data` 使用元组 `(...)`
- 列表是一个 schema 对应多行元组

## Schema

```ason
{id, name, active}
{id:int, name:str, active:bool}
```

文本模式下，类型注解是可选的。

### 常见类型注解

| 注解 | 含义 |
|------|------|
| `int` | 整数 |
| `float` | 浮点数 |
| `str` | 字符串 |
| `bool` | 布尔值 |
| `{...}` | 嵌套结构体 |
| `[type]` | 数组 |
| `<K:V>` | 映射 |

## 元组

```ason
(1, Alice, true)
```

值按 schema 顺序排列。

列表写法是一个 schema 对应多行元组：

```ason
[{id:int, name:str}]:
  (1, Alice),
  (2, Bob)
```

## 标量值

### 整数

```ason
42
-7
0
```

### 浮点数

```ason
3.14
-0.5
```

### 布尔值

```ason
true
false
```

### Null / Optional

空槽表示 null 或缺失值：

```ason
[{id:int, label:str}]:
  (1, hello),
  (2,      )
```

### 字符串

当字符串不包含保留语法字符时，可以不加引号：

```ason
Alice
hello world
```

需要保留空白或包含保留字符时使用引号：

```ason
"value with, comma"
"  leading spaces  "
"line\nbreak"
```

## 嵌套结构

```ason
[{name:str, dept:{title:str}}]:
  (Alice, (Engineering)),
  (Bob,   (Platform))
```

## 数组

```ason
[{name:str, scores:[int]}]:
  (Alice, [90, 85, 92]),
  (Bob,   [76, 88])
```

## 映射

```ason
[{name:str, attrs:<str:int>}]:
  (Alice, <age:30, score:95>)
```

## 注释

ASON 支持块注释：

```ason
/* user list schema */
[{id:int, name:str}]:
  (1, Alice),
  (2, Bob)
```

行注释不是格式的一部分。

## 单值写法

单个值可以写成单行形式：

```ason
{id:int, name:str, active:bool}:(1, Alice, true)
```

## 空白规则

- token 之间的空白会被忽略
- 不带引号的字符串会自动 trim
- 推荐使用多行美化布局，但不是必须
