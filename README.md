# Bencoder

[![Python Version](https://img.shields.io/badge/python-3.9%2B-blue.svg)](https://www.python.org/)
[![Tests](https://img.shields.io/badge/tests-pytest-green.svg)](https://docs.pytest.org/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

`Bencoder` is a **lightweight, type-safe Python module** for **Bencode encoding and decoding**, designed for use in **BitTorrent clients**.

It fully supports:

- Integers (`int`)
- Strings (`str` -> `bytes`)
- Bytes (`bytes`)
- Lists (`list`)
- Dictionaries (`dict`)
- Nested structures
- Explicit errors for invalid input
- Full unit test coverage with pytest

## Installation

```bash
git clone https://github.com/soareseng/bencoder.git
cd bencoder

python -m venv venv

# Linux / macOS
source venv/bin/activate

# Windows (PowerShell)
venv\Scripts\Activate.ps1

# Windows (cmd)
venv\Scripts\activate.bat

pip install -r requirements.txt
```

## Basic Usage

```python
from src.bencoder import Encoder, Decoder

encode = Encoder().encode

# Encode values
data = {"name": "Alice", "age": 30, "files": ["a.txt", "b.txt"]}
encoded = encode(data)
print(encoded)
# Output: b'd3:agei30e5:filesl5:a.txt5:b.txte4:name5:Alicee'

# Decode
decoded = Decoder(encoded).decode()
print(decoded)
# Output: {b'age': 30, b'files': [b'a.txt', b'b.txt'], b'name': b'Alice'}
```

Note: all strings are converted to bytes after decoding, in accordance with the Bencode specification.

## Typing

All methods use the recursive type `BencodeValue`:

```python
from typing import Union, List, Dict, TypeAlias

BencodeValue: TypeAlias = Union[int, bytes, List["BencodeValue"], Dict[bytes, "BencodeValue"]]
```

- `Encoder.encode(value: BencodeValue) -> bytes`
- `Decoder.decode() -> BencodeValue`

This ensures type safety even in nested structures and works with `mypy --strict`.

## Errors

The module defines explicit exceptions for better robustness:

- `BencodeEncodeError`: unsupported type or invalid dictionary key
- `BencodeDecodeError`: malformed input, invalid string, or unterminated integer

```python
from src.bencoder import BencodeEncodeError, BencodeDecodeError, Encoder

try:
    Encoder().encode({1: "invalid"})
except BencodeEncodeError as e:
    print(e)
```

## Testing

The module comes with **comprehensive pytest tests**, covering:

- All supported types (`int`, `bytes`, `str`, `list`, `dict`)

- Nested structures of arbitrary depth

- Invalid inputs

- Round-trip encode -> decode validation

- Post-decode type checks (`bytes`, `int`, `list`, `dict`)

```bash
# Run all tests
pytest -v tests/
```

## Contributing

Pull requests are welcome!
Enhancements such as performance optimizations, streaming decode, or additional tests are highly encouraged.

- Open issues for bugs or feature requests
- Fork the repo, implement, and submit a PR

## License

MIT License © [Soares W. / soareseng]
