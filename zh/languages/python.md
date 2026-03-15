# Python 指南

Python 实现提供一个编译扩展模块，公开一组较小而稳定的文本与二进制 API。

## 要求

- 推荐 Python 3.13+
- 构建和运行必须使用同一个 Python 版本

## 安装

```bash
cd ason-py
python3 -m pip install -e .
```

或直接在项目内构建扩展：

```bash
cd ason-py
python3 setup.py build_ext --inplace
```

## 文本 API

```python
import ason

users = [
    {"id": 1, "name": "Alice", "active": True},
    {"id": 2, "name": "Bob", "active": False},
]

text = ason.encode(users)
typed = ason.encodeTyped(users)
pretty = ason.encodePrettyTyped(users)

restored = ason.decode(typed)
assert restored == users
```

`encode()` 会自动推断 schema 并输出 untyped 文本。`encodeTyped()` 会输出 typed schema，更适合保真 round-trip。

对 untyped 文本使用 `decode()` 时，由于 schema 不带类型信息，返回值里的字段会以字符串形式出现。

## 二进制 API

```python
blob = ason.encodeBinary(users)
restored = ason.decodeBinary(blob, "[{id@int, name@str, active@bool}]")
```

二进制编码会在内部推断 schema，但二进制解码当前仍需要显式传入 schema。

## 运行测试

```bash
cd ason-py
python3 -m pytest tests -v
```

## 运行示例

```bash
cd ason-py
python3 examples/basic.py
python3 examples/bench.py
python3 examples/complex.py
```

## 性能说明

Python 的优势更多来自重复结构数据、文本更紧凑和 schema 复用，而不是解释器层面的极限吞吐。实现级说明见 [benchmark notes](/zh/reference/benchmark-notes)。
