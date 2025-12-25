# Project Configuration

Patterns for configuring pyproject.toml and working with existing projects.

## pyproject.toml with uv

```toml
[project]
name = "my-project"
version = "0.1.0"
description = "My awesome project"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "pydantic>=2.0.0",
    "httpx>=0.25.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=7.4.0",
    "pytest-cov>=4.1.0",
    "ruff>=0.1.0",
]
docs = [
    "sphinx>=7.0.0",
    "sphinx-rtd-theme>=1.3.0",
]

[build-system]
requires = ["uv_build>=0.9.18,<0.10.0"]
build-backend = "uv_build"

[tool.uv]
dev-dependencies = [
    # Additional dev dependencies managed by uv
]

[tool.uv.sources]
# Custom package sources
my-package = { git = "https://github.com/user/repo.git" }

[tool.uv.pip]
# Disable build isolation for specific packages (may be necessary for some ML libraries)
# no-build-isolation-package = ["adam-atan2"]
```

## Working with Existing Projects

### Migrate from requirements.txt

```bash
# Add all dependencies from requirements.txt
uv add -r requirements.txt

# Or use pip-compatible install
uv pip install -r requirements.txt
```

### Migrate from Poetry

```bash
# If you already have pyproject.toml, just sync
uv sync

# uv reads [project] and [tool.poetry] sections
```

### Export to requirements.txt

```bash
# Basic export
uv pip freeze > requirements.txt

# Export with hashes
uv pip freeze --require-hashes > requirements.txt

# Export from lockfile
uv export --format requirements-txt > requirements.txt
```

## Project Initialization

```bash
# Create new project
uv init my-project
cd my-project

# Creates:
# - .python-version (Python version)
# - pyproject.toml (project config)
# - README.md
# - .gitignore

# Initialize in current directory
uv init .
```
