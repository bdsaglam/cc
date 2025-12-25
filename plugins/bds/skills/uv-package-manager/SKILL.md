---
name: uv-package-manager
description: Master the uv package manager for fast Python dependency management, virtual environments, and modern Python project workflows. Use when setting up Python projects, managing dependencies, or optimizing Python development workflows with uv.
---

# UV Package Manager

Comprehensive guide to using uv, an extremely fast Python package installer and resolver written in Rust, for modern Python project management and dependency workflows.

## When to Use This Skill

- Setting up new Python projects quickly
- Managing Python dependencies faster than pip
- Creating and managing virtual environments
- Installing Python interpreters
- Resolving dependency conflicts efficiently
- Migrating from pip/pip-tools/poetry
- Speeding up CI/CD pipelines
- Managing monorepo Python projects
- Working with lockfiles for reproducible builds
- Optimizing Docker builds with Python dependencies

## Core Concepts

- **Ultra-fast package installer**: 10-100x faster than pip
- **Written in Rust**: Leverages Rust's performance
- **Drop-in pip replacement**: Compatible with pip workflows
- **Virtual environment manager**: Create and manage venvs
- **Python installer**: Download and manage Python versions
- **Resolver**: Advanced dependency resolution
- **Lockfile support**: Reproducible installations

## Installation

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Using Homebrew (macOS)
brew install uv

# Verify
uv --version
```

## Quick Start

```bash
# Create new project
uv init my-project
cd my-project

# Add dependencies
uv add requests pandas

# Add dev dependencies
uv add --dev pytest ruff

# Run Python script
uv run python app.py

# Run tests
uv run pytest
```

## Essential Commands

```bash
# Project management
uv init [PATH]              # Initialize project
uv add PACKAGE              # Add dependency
uv add --dev PACKAGE        # Add dev dependency
uv remove PACKAGE           # Remove dependency
uv sync                     # Install dependencies
uv lock                     # Create/update lockfile

# Virtual environments
uv venv [PATH]              # Create venv
uv venv --python 3.12       # Create with specific Python
uv venv --allow-existing    # Allow existing venv
uv run COMMAND              # Run in venv (no activation needed)
uv run --active COMMAND     # Run in active venv

# Python management
uv python install VERSION   # Install Python
uv python list              # List installed Pythons
uv python pin VERSION       # Pin Python version

# Package installation (pip-compatible)
uv pip install PACKAGE      # Install package
uv pip install -r requirements.txt
uv pip freeze               # List installed
uv pip list                 # List packages

# Utility
uv cache clean              # Clear cache
uv --version                # Show version
```

## References

Consult these files for detailed patterns based on the task:

- **Virtual environments**: See `references/venv-and-run.md` for venv creation and uv run usage
- **Package management**: See `references/dependencies.md` for add/remove/upgrade/lock workflows
- **Python versions**: See `references/python-versions.md` for installing and pinning Python
- **Project setup**: See `references/project-config.md` for pyproject.toml and existing projects
- **CI/CD and Docker**: See `references/advanced-workflows.md` for deployment pipelines
- **Inline scripts**: See `references/inline-scripts.md` for standalone scripts with embedded deps
- **Performance**: See `references/performance.md` for cache and optimization
- **Migration**: See `references/migrations.md` when moving from pip, poetry, or pip-tools
- **Troubleshooting**: See `references/troubleshooting.md` for common issues and fixes

## Best Practices Summary

1. **Use uv for all new projects** - Start with `uv init`
2. **Commit lockfiles** - Ensure reproducible builds with `uv.lock`
3. **Pin Python versions** - Use `.python-version`
4. **Use uv run** - Avoid manual venv activation
5. **Use --frozen in CI** - Exact reproduction with `uv sync --frozen`
6. **Keep uv updated** - Fast-moving project with frequent improvements

## Resources

- **Official documentation**: https://docs.astral.sh/uv/
- **GitHub repository**: https://github.com/astral-sh/uv
