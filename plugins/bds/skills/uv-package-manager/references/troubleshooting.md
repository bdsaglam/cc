# Troubleshooting

Common issues and solutions when using uv.

## uv not found

```bash
# Solution: Add to PATH
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Or reinstall
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## Wrong Python Version

```bash
# Solution: Pin version explicitly
uv python pin 3.12
uv venv --python 3.12

# Verify
uv run python --version
```

## Dependency Conflict

```bash
# Solution: Check resolution with verbose output
uv lock --verbose

# Try upgrading conflicting packages
uv lock --upgrade-package conflicting-package

# Check dependency tree
uv tree
```

## Cache Issues

```bash
# Solution: Clear cache
uv cache clean

# Verify cache location
uv cache dir
```

## Lockfile Out of Sync

```bash
# Solution: Regenerate lockfile
uv lock --upgrade

# Or force sync
uv sync --refresh
```

## ML Library Build Failures

Some ML libraries (e.g., adam-atan2) may fail with build isolation:

```toml
# Solution: Add to pyproject.toml
[tool.uv.pip]
no-build-isolation-package = ["adam-atan2"]
```

## Permission Issues

```bash
# Solution: Check venv ownership
ls -la .venv/

# Recreate venv
rm -rf .venv
uv venv
uv sync
```

## Network Issues

```bash
# Use offline mode if packages are cached
uv sync --frozen --offline

# Check proxy settings
echo $HTTP_PROXY $HTTPS_PROXY
```

## Package Not Found

```bash
# Check package name spelling
uv search package-name

# Try with different index
uv add package --index-url https://pypi.org/simple/
```

## Slow First Install

First install may be slow due to cache population. Subsequent installs will be faster.

```bash
# Check if using cache
uv sync --verbose
```

## Getting Help

```bash
# Show uv version
uv --version

# Show help
uv --help
uv add --help

# Check official docs
# https://docs.astral.sh/uv/
```
