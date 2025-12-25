# Virtual Environment Management

Patterns for creating virtual environments and running commands with uv.

## Creating Virtual Environments

```bash
# Create virtual environment with uv
uv venv

# Create with specific Python version
uv venv --python 3.12

# Create with custom name
uv venv my-env

# Create with system site packages
uv venv --system-site-packages

# Specify location
uv venv /path/to/venv
```

## Activating Virtual Environments

```bash
# Linux/macOS
source .venv/bin/activate

# Or use uv run (no activation needed - recommended)
uv run python script.py
uv run pytest
```

## Using uv run

The `uv run` command executes commands within the virtual environment without requiring activation.

```bash
# Run Python script (auto-activates venv)
uv run python app.py

# Run installed CLI tool
uv run pytest
uv run ruff check .

# Run with specific Python version
uv run --python 3.11 python script.py

# Pass arguments
uv run python script.py --arg value

# Run module
uv run python -m http.server 8000
```

## Benefits of uv run

1. **No activation needed** - Just prefix your command
2. **Auto-creates venv** - Creates `.venv` if it doesn't exist
3. **Auto-installs deps** - Syncs dependencies before running
4. **Cleaner scripts** - No need for activation in CI/CD
5. **Python version aware** - Respects `.python-version`
