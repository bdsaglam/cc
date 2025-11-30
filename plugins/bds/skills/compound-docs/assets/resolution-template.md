---
module: [module.path or "project" for project-wide]
date: [YYYY-MM-DD]
problem_type: [import_error|type_error|runtime_error|test_failure|performance_issue|database_issue|security_issue|api_error|async_issue|config_error|logic_error|dependency_issue|developer_experience|documentation_gap]
component: [orm_model|api_endpoint|api_router|service_layer|repository|background_task|database|cache|authentication|serializer|middleware|cli|testing|configuration|logging|external_api|websocket|graphql]
symptoms:
  - [Observable symptom 1 - specific error message or traceback]
  - [Observable symptom 2 - what user actually saw/experienced]
root_cause: [circular_import|missing_dependency|wrong_import_order|missing_eager_load|missing_index|wrong_async_usage|session_scope|connection_pool|race_condition|memory_leak|config_error|type_mismatch|serialization_error|cache_invalidation|logic_error|test_isolation|missing_validation|missing_permission|version_incompatibility|missing_migration]
python_version: [3.11 - optional]
framework: [FastAPI 0.104.1 - optional]
resolution_type: [code_fix|migration|config_change|test_fix|dependency_update|dependency_pin|environment_setup|refactor|documentation_update|type_annotation]
severity: [critical|high|medium|low]
tags: [keyword1, keyword2, keyword3]
---

# Troubleshooting: [Clear Problem Title]

## Problem
[1-2 sentence clear description of the issue and what the user experienced]

## Environment
- Module: [Name or "project-wide"]
- Python Version: [e.g., 3.11.5]
- Framework: [e.g., FastAPI 0.104.1, Django 5.0, Flask 3.0]
- Affected Component: [e.g., "User model", "Auth endpoint", "Celery task"]
- Date: [YYYY-MM-DD when this was solved]

## Symptoms
- [Observable symptom 1 - what the user saw/experienced]
- [Observable symptom 2 - error messages, tracebacks, unexpected behavior]
- [Continue as needed - be specific]

## What Didn't Work

**Attempted Solution 1:** [Description of what was tried]
- **Why it failed:** [Technical reason this didn't solve the problem]

**Attempted Solution 2:** [Description of second attempt]
- **Why it failed:** [Technical reason]

[Continue for all significant attempts that DIDN'T work]

[If nothing else was attempted first, write:]
**Direct solution:** The problem was identified and fixed on the first attempt.

## Solution

[The actual fix that worked - provide specific details]

**Code changes** (if applicable):
```python
# Before (broken):
[Show the problematic code]

# After (fixed):
[Show the corrected code with explanation]
```

**Database migration** (if applicable):
```python
# Migration change (Alembic/Django):
[Show what was changed in the migration]
```

**Configuration changes** (if applicable):
```python
# settings.py / .env / pyproject.toml:
[Show configuration changes]
```

**Commands run** (if applicable):
```bash
# Steps taken to fix:
[Commands or actions]
```

## Why This Works

[Technical explanation of:]
1. What was the ROOT CAUSE of the problem?
2. Why does the solution address this root cause?
3. What was the underlying issue (API misuse, configuration error, version issue, etc.)?

[Be detailed enough that future developers understand the "why", not just the "what"]

## Prevention

[How to avoid this problem in future development:]
- [Specific coding practice, check, or pattern to follow]
- [What to watch out for]
- [How to catch this early (e.g., linting rules, type checking, tests)]

## Related Issues

[If any similar problems exist in docs/solutions/, link to them:]
- See also: [another-related-issue.md](../category/another-related-issue.md)
- Similar to: [related-problem.md](../category/related-problem.md)

[If no related issues, write:]
No related issues documented yet.
