---
name: project-structure
description: Use when designing or refactoring a FastAPI project layout, defining per-domain package structure, or standardizing cross-module import conventions for multi-domain apps.
---

# Project Structure

## Overview
Provide a scalable, domain-oriented FastAPI layout and module conventions for multi-domain codebases.

## When to Use
- Creating a new FastAPI app with multiple domains/modules
- Refactoring a file-type-based layout (routers/, models/, crud/) that no longer scales
- Standardizing module boundaries and cross-module imports

## Quick Reference
| Topic | Guidance |
| --- | --- |
| Domains | Put each domain under `src/` with its own module set |
| App root | Keep `main.py`, `config.py`, `database.py` at `src/` root |
| Imports | Use explicit module imports across domains |
| Tests | Mirror domain structure under `tests/` |

## Implementation
1. Create domain packages under `src/`.
2. Within each domain, use the standard file set (`router.py`, `schemas.py`, `models.py`, `service.py`, `dependencies.py`, `constants.py`, `config.py`, `utils.py`, `exceptions.py`).
3. Keep global modules at `src/` root (config, database, exceptions, pagination).
4. For the full layout and examples, read `references/project-structure.md`.

## Example
```python
from src.posts.constants import ErrorCode as PostsErrorCode
from src.notifications import service as notification_service
```

## Common Mistakes
- Grouping by file type across the whole app in large monoliths
- Hiding cross-domain dependencies via relative imports
- Mixing global config with domain config

## References
- `references/project-structure.md`
