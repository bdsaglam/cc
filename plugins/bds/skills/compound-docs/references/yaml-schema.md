# YAML Frontmatter Schema

**See `schema.yaml` for the complete schema specification.**

## Required Fields

- **module** (string): Module/package path (e.g., "user_service", "api.endpoints") or "project" for project-wide issues
- **date** (string): ISO 8601 date (YYYY-MM-DD)
- **problem_type** (enum): One of [import_error, type_error, runtime_error, test_failure, performance_issue, database_issue, security_issue, api_error, async_issue, config_error, logic_error, dependency_issue, developer_experience, documentation_gap]
- **component** (enum): One of [orm_model, api_endpoint, api_router, service_layer, repository, background_task, database, cache, authentication, serializer, middleware, cli, testing, configuration, logging, external_api, websocket, graphql]
- **symptoms** (array): 1-5 specific observable symptoms
- **root_cause** (enum): One of [circular_import, missing_dependency, wrong_import_order, missing_eager_load, missing_index, wrong_async_usage, session_scope, connection_pool, race_condition, memory_leak, config_error, type_mismatch, serialization_error, cache_invalidation, logic_error, test_isolation, missing_validation, missing_permission, version_incompatibility, missing_migration]
- **resolution_type** (enum): One of [code_fix, migration, config_change, test_fix, dependency_update, dependency_pin, environment_setup, refactor, documentation_update, type_annotation]
- **severity** (enum): One of [critical, high, medium, low]

## Optional Fields

- **python_version** (string): Python version in X.Y or X.Y.Z format
- **framework** (string): Framework name and version (e.g., "FastAPI 0.104.1")
- **tags** (array): Searchable keywords (lowercase, hyphen-separated)

## Validation Rules

1. All required fields must be present
2. Enum fields must match allowed values exactly (case-sensitive)
3. symptoms must be YAML array with 1-5 items
4. date must match YYYY-MM-DD format
5. python_version (if provided) must match X.Y or X.Y.Z format
6. tags should be lowercase, hyphen-separated

## Example

```yaml
---
module: user_service
date: 2025-11-30
problem_type: performance_issue
component: orm_model
symptoms:
  - "N+1 query when loading user profiles"
  - "API endpoint taking >5 seconds"
root_cause: missing_eager_load
python_version: "3.11"
framework: "FastAPI 0.104.1"
resolution_type: code_fix
severity: high
tags: [n-plus-one, eager-loading, sqlalchemy, performance]
---
```

## Category Mapping

Based on `problem_type`, documentation is filed in:

- **import_error** → `docs/solutions/import-errors/`
- **type_error** → `docs/solutions/type-errors/`
- **runtime_error** → `docs/solutions/runtime-errors/`
- **test_failure** → `docs/solutions/test-failures/`
- **performance_issue** → `docs/solutions/performance-issues/`
- **database_issue** → `docs/solutions/database-issues/`
- **security_issue** → `docs/solutions/security-issues/`
- **api_error** → `docs/solutions/api-errors/`
- **async_issue** → `docs/solutions/async-issues/`
- **config_error** → `docs/solutions/config-errors/`
- **logic_error** → `docs/solutions/logic-errors/`
- **dependency_issue** → `docs/solutions/dependency-issues/`
- **developer_experience** → `docs/solutions/developer-experience/`
- **documentation_gap** → `docs/solutions/documentation-gaps/`
