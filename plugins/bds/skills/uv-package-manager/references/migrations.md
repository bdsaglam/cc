# Migration Guide

Patterns for migrating from pip, poetry, and pip-tools to uv.

## From pip + requirements.txt

### Before

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### After

```bash
# Option 1: Use pip-compatible interface
uv venv
uv pip install -r requirements.txt

# Option 2: Convert to project (recommended)
uv init
uv add -r requirements.txt
```

## From Poetry

### Before

```bash
poetry install
poetry add requests
```

### After

```bash
# Keep existing pyproject.toml
# uv reads [project] and [tool.poetry] sections
uv sync
uv add requests
```

Poetry's pyproject.toml is compatible - just start using uv commands.

## From pip-tools

### Before

```bash
pip-compile requirements.in
pip-sync requirements.txt
```

### After

```bash
uv lock
uv sync --frozen
```

## From Conda

```bash
# Export conda environment
conda list --export > requirements.txt

# Create uv project
uv init
uv add -r requirements.txt

# Note: Some conda packages may need alternatives
# e.g., cuda packages need different handling
```

## Generating requirements.txt

For tools that still need requirements.txt:

```bash
# Basic export
uv pip freeze > requirements.txt

# Export with hashes (for security)
uv pip freeze --require-hashes > requirements.txt

# Export from lockfile
uv export --format requirements-txt > requirements.txt
```

## Gradual Migration

You can use uv alongside existing tools:

```bash
# Keep requirements.txt for compatibility
# Use uv for speed

# Install from requirements.txt
uv pip install -r requirements.txt

# But prefer adding to pyproject.toml
uv add new-package
```
