# Python Guide

The Python implementation exposes a compiled extension module with a small text and binary API surface.

## Requirements

- Python 3.13+ recommended
- Build and run with the same Python version

## Install

```bash
cd ason-py
python3 -m pip install -e .
```

Or build the extension in place:

```bash
cd ason-py
python3 setup.py build_ext --inplace
```

## Text API

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

`encode()` infers schema and emits untyped text. `encodeTyped()` emits typed schema and should be preferred for type-preserving round-trips.

When decoding untyped text with `decode()`, values are returned as strings because the schema does not carry types.

## Binary API

```python
blob = ason.encodeBinary(users)
restored = ason.decodeBinary(blob, "[{id:int, name:str, active:bool}]")
```

Binary encoding infers schema internally. Binary decoding still needs an explicit schema.

## Run Tests

```bash
cd ason-py
python3 -m pytest tests -v
```

## Run Examples

```bash
cd ason-py
python3 examples/basic.py
python3 examples/bench.py
python3 examples/complex.py
```

## Performance Notes

Python benefits most on repetitive structured payloads. Expect bigger wins from smaller text payloads and inferred schema reuse than from raw interpreter speed alone. See [benchmark notes](/reference/benchmark-notes).
