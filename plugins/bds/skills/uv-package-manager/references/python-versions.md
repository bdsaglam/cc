# Python Version Management

Patterns for installing and managing Python versions with uv.

## Installing Python Versions

```bash
# Install Python version
uv python install 3.12

# Install multiple versions
uv python install 3.11 3.12 3.13

# Install latest version
uv python install

# List installed versions
uv python list

# Find available versions
uv python list --all-versions
```

## Setting Python Version

```bash
# Pin Python version for project (creates .python-version)
uv python pin 3.12

# Use specific Python version for command
uv --python 3.11 run python script.py

# Create venv with specific version
uv venv --python 3.12
```

## .python-version File

When you run `uv python pin 3.12`, it creates a `.python-version` file:

```
3.12
```

This file is respected by:
- `uv run` - Uses this version
- `uv venv` - Creates venv with this version
- `uv sync` - Ensures compatibility

## Multi-Version Testing

```bash
# Test with different Python versions
uv --python 3.10 run pytest
uv --python 3.11 run pytest
uv --python 3.12 run pytest

# Or use tox/nox with uv
uv run tox
```
