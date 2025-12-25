---
name: Python
description: This skill should be used when the user asks to "write Python code", "refactor Python", "create pytest tests", "add type hints", "create an enum", "create a singleton", "handle exceptions", "configure logging", "async Python", "pydantic settings", or needs guidance on Python coding conventions, library choices, or testing patterns.
---

# Python Best Practices

## General Guidelines

- Create modular functions with good abstractions when refactoring
- Activate virtual environment before running Python code: `source .venv/bin/activate` or use `uv run`
- Prefer async/await patterns when possible

## Code Style

- Prefer list/dict comprehension over map/reduce
- Use `pathlib` for path operations instead of `os` module
- Use `list`, `dict` for type hints instead of `List`, `Dict`
- Use `T | None` instead of `Optional[T]`

## Singleton Pattern

Apply `@cache` decorator for singleton objects:

```python
from functools import cache

@cache
def get_singleton_instance():
    return MySingleton()
```

## Enum Conventions

Define enums with lowercase values:

```python
from enum import Enum

class Status(str, Enum):
    pending = "pending"
    completed = "completed"
    failed = "failed"
```

## Exception Handling

Use base + specific exception pattern per module:

```python
class UserServiceError(Exception):
    """Base exception for user service."""

class UserNotFoundError(UserServiceError):
    """Raised when user is not found."""

class UserValidationError(UserServiceError):
    """Raised when user data is invalid."""
```

## Library Preferences

| Purpose | Library |
|---------|---------|
| Testing | `pytest` |
| CLI apps | `typer` |
| Pretty printing | `rich` |
| HTTP requests | `httpx` |
| Data validation | `pydantic` |
| Configuration | `pydantic-settings` |
| Logging | `structlog` |
| Unique IDs | `uuid6` (use `uuid7()`) |
| Environment variables | `python-dotenv` |
| Linting/formatting | `ruff` |
| Pre-commit hooks | `pre-commit` |


## Pydantic Conventions

- Always use Pydantic models for data structures (not dataclasses)
- Use `model_validate()` for parsing external data
- Enable `strict=True` for type safety

```python
from pydantic import BaseModel, ConfigDict

class User(BaseModel):
    model_config = ConfigDict(strict=True)

    name: str
    email: str

# Parsing external data
data = {"name": "Alice", "email": "alice@example.com"}
user = User.model_validate(data)
```

## Configuration

Production code: use `pydantic-settings` for env var management:

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    api_key: str
    debug: bool = False

settings = Settings()
```

Scripts: access via `os.environ` directly, use `python-dotenv` to load `.env`:

```python
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.environ["API_KEY"]
```

## Async Patterns

Prefer async httpx client and async context managers:

```python
import httpx

async def fetch_data(url: str) -> dict:
    async with httpx.AsyncClient() as client:
        response = await client.get(url)
        return response.json()
```

## Logging

Use structlog for structured logging with context binding:

```python
import structlog

log = structlog.get_logger()

# Bind context - returns new immutable logger
log = log.bind(user_id=123, request_id="req-abc")

# All subsequent logs include bound context
log.info("page_view", page="/dashboard")
# Output: user_id=123 request_id=req-abc page=/dashboard event=page_view
```

Use `bound_contextvars` for temporary context (context manager or decorator):

```python
from structlog.contextvars import bound_contextvars, clear_contextvars

# As context manager - context auto-restored after block
with bound_contextvars(request_id="req-123", user_id=42):
    log.info("processing")  # includes request_id and user_id
log.info("done")  # context restored

# As decorator
@bound_contextvars(component="auth")
def authenticate(user):
    log.info("auth_attempt")  # includes component=auth
```

Common pattern - clear context at request start:

```python
def handle_request(request):
    clear_contextvars()  # clean slate
    structlog.contextvars.bind_contextvars(
        request_id=request.id,
        user_id=request.user.id,
    )
    # all logs in request lifecycle include this context
```

## pytest Conventions

- Apply `@pytest.mark.parametrize` for testing multiple cases
- Write function-based tests (`def test_happy_path_something()`) rather than class-based tests

```python
import pytest

@pytest.mark.parametrize("input,expected", [
    ("hello", "HELLO"),
    ("world", "WORLD"),
])
def test_uppercase(input, expected):
    assert input.upper() == expected


def test_happy_path_user_creation():
    user = create_user(name="Alice", email="alice@example.com")
    assert user.name == "Alice"


def test_error_case_invalid_email():
    with pytest.raises(ValueError):
        create_user(name="Alice", email="invalid")
```
