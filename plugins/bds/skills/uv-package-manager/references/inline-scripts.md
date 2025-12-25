# Standalone Scripts with Inline Dependencies

Patterns for creating Python scripts with embedded dependency declarations.

## Declaring Script Dependencies

The inline metadata format allows dependencies to be declared directly in the script.

Use `uv add --script` to declare dependencies:

```bash
uv add --script example.py 'requests<3' 'rich'
```

This adds a `script` section at the top of the script:

```python
# /// script
# dependencies = [
#   "requests<3",
#   "rich",
# ]
# ///

import requests
from rich.pretty import pprint

resp = requests.get("https://peps.python.org/api/peps.json")
data = resp.json()
pprint([(k, v["title"]) for k, v in data.items()][:10])
```

## Running Scripts

uv automatically creates an environment with the necessary dependencies:

```bash
uv run example.py
```

**Important**: When using inline script metadata, even if `uv run` is used in a project, the project's dependencies will be ignored.

## Specifying Python Version

Scripts can also specify Python version requirements:

```python
# /// script
# requires-python = ">=3.12"
# dependencies = []
# ///

# Use syntax added in Python 3.12
type Point = tuple[float, float]
print(Point)
```

**Note**: The `dependencies` field must be provided even if empty.

`uv run` will search for and use the required Python version, downloading it if not installed.

## Creating Executable Scripts with Shebang

Add a shebang to make scripts executable without `uv run`:

```python
#!/usr/bin/env -S uv run --script

print("Hello, world!")
```

Make executable and run:

```bash
chmod +x greet
./greet
```

## Full Example with Dependencies and Shebang

```python
#!/usr/bin/env -S uv run --script
# /// script
# requires-python = ">=3.12"
# dependencies = ["httpx"]
# ///

import httpx

print(httpx.get("https://example.com"))
```

## Use Cases

1. **Quick utilities** - One-off scripts without project setup
2. **Shareable scripts** - Single file with all deps declared
3. **System tools** - Executable scripts on PATH
4. **CI/CD helpers** - Self-contained automation scripts
