# Syntax Reference

Complete reference for the ASON text format.

## Document Structure

Every document has this shape:

```text
schema ":" data
```

- `schema` uses `{...}`
- `data` uses tuples `(...)`
- a list uses one schema with multiple tuples

## Schema

```ason
{id, name, active}
{id@int, name@str, active@bool}
```

Type annotations are optional in text mode.

### Common Type Annotations

| Annotation | Meaning |
|-----------|---------|
| `int` | Integer |
| `float` | Floating-point number |
| `str` | String |
| `bool` | Boolean |
| `{...}` | Nested struct |
| `[type]` | Array |
| `[{...}]` | Array of structs |

## Tuples

```ason
(1, Alice, true)
```

Values follow schema order.

Lists use one schema and many tuples:

```ason
[{id@int, name@str}]:
  (1, Alice),
  (2, Bob)
```

## Scalar Values

### Integer

```ason
42
-7
0
```

### Float

```ason
3.14
-0.5
```

### Boolean

```ason
true
false
```

### Null / Optional

An empty slot means null / absent:

```ason
[{id@int, label@str}]:
  (1, hello),
  (2,      )
```

### Strings

Unquoted strings are allowed when they do not contain reserved syntax characters:

```ason
Alice
hello world
```

Use quotes when you need to preserve whitespace or include reserved characters:

```ason
"value with, comma"
"  leading spaces  "
"line\nbreak"
```

## Nested Structs

```ason
[{name@str, dept@{title@str}}]:
  (Alice, (Engineering)),
  (Bob,   (Platform))
```

## Arrays

```ason
[{name@str, scores@[int]}]:
  (Alice, [90, 85, 92]),
  (Bob,   [76, 88])
```

## Keyed Entry Lists

```ason
[{name@str, attrs@[{key@str, value@int}]}]:
  (Alice, [(age, 30), (score, 95)])
```

## Comments

ASON supports block comments:

```ason
/* schema for a user list */
[{id@int, name@str}]:
  (1, Alice),
  (2, Bob)
```

Line comments are not part of the format.

## Single-Value Form

A single value can be written on one line:

```ason
{id@int, name@str, active@bool}:(1, Alice, true)
```

## Whitespace

- whitespace between tokens is ignored
- unquoted strings are trimmed
- pretty multi-line layout is optional but recommended
