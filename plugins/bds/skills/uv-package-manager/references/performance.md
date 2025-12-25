# Performance Optimization

Patterns for cache management and performance optimization with uv.

## Global Cache

uv automatically uses a global cache:

- **Linux**: `~/.cache/uv`
- **macOS**: `~/Library/Caches/uv`

```bash
# Show cache location
uv cache dir

# Clear cache
uv cache clean

# Check cache size
du -sh $(uv cache dir)
```

## Offline Mode

Install from cache only without network access:

```bash
# Install from cache only
uv pip install --offline package

# Sync from lockfile offline
uv sync --frozen --offline
```

## Frozen Installs

Use `--frozen` to skip resolution and install exact versions:

```bash
# Install exact versions from lockfile
uv sync --frozen

# Faster CI builds
uv sync --frozen --no-dev
```

## Performance Tips

### In CI/CD

```bash
# Use frozen installs (skips resolution)
uv sync --frozen

# Cache uv cache directory
# GitHub Actions example with setup-uv:
# uses: astral-sh/setup-uv@v2
# with:
#   enable-cache: true

# Use offline mode when deps are cached
uv sync --frozen --offline
```

### Local Development

```bash
# uv parallelizes operations automatically

# Reuse cache across environments
# uv shares cache globally by default

# Use lockfiles to skip resolution
uv sync --frozen
```

## Speed Comparison

| Operation | pip | uv | Speedup |
|-----------|-----|-----|---------|
| Install packages | ~30s | ~2s | 10-15x |
| Create venv + install | ~35s | ~3s | 10x |
| Lock dependencies | ~15s | ~2s | 7-8x |

## Disk Space Efficiency

uv uses hard links to share packages across environments:

- Packages downloaded once
- Shared across all projects
- Minimal disk usage per project
