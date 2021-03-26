# pylic - Python license checker [![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/sandrochuber/pylic/blob/main/LICENSE) [![PyPI version](https://badge.fury.io/py/pylic.svg)](https://badge.fury.io/py/pylic/) [![Codecov](https://codecov.io/gh/sandrochuber/pylic//branch/main/graph/badge.svg)](https://codecov.io/gh/sandrochuber/pylic/)

Reads the pyproject.toml file and checks all installed licenses recursively.

Principles:
- Every license has to be allowed explicitly (case-insensitive comparison).
- All packages without license are considered unsafe and have to be listed as such.

## Installation

```sh
pip install pylic
```

## Configuration

`pylic` needs be run in the directory where your `pyproject.toml` file is located. You can configure
- `safe_licenses`: All licenses you concider safe for usage. The string comparison is case-insensitive.
- `unsafe_packages`: If you rely on a package that does not come with a license you have to explicitly list it as such.

```toml
[tool.pylic]
safe_licenses = [
    "Apache Software License",
    "Apache License 2.0",
    "MIT License",
    "Python Software Foundation License",
    "Mozilla Public License 2.0 (MPL 2.0)",
]
unsafe_packages = [
    "unlicensedPackage",
]
```

## Usage Example

Create a venv to start with a clean ground and activate it

```sh
python -m venv .venv
source .venv/bin/activate
```

Activate the venv and install `pylic` and create an empty `pyproject.toml`

```sh
pip install pylic
touch pyproject.toml
```

Run pylic

```sh
pylic
```

The output will be similar to

```sh
Found unsafe packages:
  pkg_resources
Found unsafe licenses:
  pip: MIT License
  zipp: MIT License
  toml: MIT License
  setuptools: MIT License
  importlib-metadata: Apache Software License
  typing-extensions: Python Software Foundation License
  pylic: MIT License
```

The return code of `pylic` is in this case non-zero

```sh
echo $? # prints 1
```

As these licenses and packages are all ok we can configure `pylic` accordingly

```sh
cat <<EOT >> pyproject.toml
[tool.pylic]
safe_licenses = ["Apache Software License", "MIT License", "Python Software Foundation License"]
unsafe_packages = ["pkg_resources"]
EOT
```

The output now reveals a successful validation

```sh
All licenses ok
```

Also the return code now signals that all is good

```sh
echo $? # prints 0
```

## Development

Required tools:
- Poetry (https://python-poetry.org/)
- GitHub cli (https://github.com/cli/cli)

Creating a new release is as simple as:
- Update `version` in the pyproject.toml file.
- `poetry run task release vx.x.x`.
