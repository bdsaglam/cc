# Dependency Management

Patterns for adding, removing, upgrading, and locking dependencies with uv.

## Adding Dependencies

```bash
# Add package (adds to pyproject.toml)
uv add requests

# Add with version constraint
uv add "django>=4.0,<5.0"

# Add multiple packages
uv add numpy pandas matplotlib

# Add dev dependency
uv add --dev pytest pytest-cov

# Add optional dependency group
uv add --optional docs sphinx

# Add from git
uv add git+https://github.com/user/repo.git

# Add from git with specific ref
uv add git+https://github.com/user/repo.git@v1.0.0

# Add from local path
uv add ./local-package

# Add editable local package
uv add -e ./local-package

# Add from requirements.txt
uv add -r requirements.txt
```

## Removing Dependencies

```bash
# Remove package
uv remove requests

# Remove dev dependency
uv remove --dev pytest

# Remove multiple packages
uv remove numpy pandas matplotlib
```

## Upgrading Dependencies

```bash
# Upgrade specific package
uv add --upgrade requests

# Upgrade all packages
uv sync --upgrade

# Show outdated packages
uv tree --outdated
```

## Locking Dependencies

```bash
# Generate uv.lock file
uv lock

# Update lock file
uv lock --upgrade

# Lock specific package only
uv lock --upgrade-package requests

# Check if lockfile is up to date
uv lock --check

# Install from lockfile (exact versions)
uv sync --frozen

# Export lockfile to requirements.txt
uv export --format requirements-txt > requirements.txt

# Export with hashes for security
uv export --format requirements-txt --hash > requirements.txt
```

## Syncing Dependencies

```bash
# Install all dependencies from pyproject.toml
uv sync

# Install with dev dependencies
uv sync --dev

# Install all extras
uv sync --all-extras

# Install from locked versions only
uv sync --frozen

# Install without dev dependencies (production)
uv sync --no-dev
```
