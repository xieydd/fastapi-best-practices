---
name: dependencies
description: Use when validating requests with database lookups, chaining authentication/authorization checks, or refactoring repeated FastAPI dependency logic.
---

# Dependencies

## Overview
Use dependencies for request validation, reuse, and composition instead of duplicating checks in routes.

## When to Use
- Validation requires DB or external calls
- Multiple endpoints repeat the same checks
- You need to compose auth/ownership/role checks

## Quick Reference
| Scenario | Guidance |
| --- | --- |
| Repeated checks | Move into dependencies |
| Auth + ownership | Chain dependencies |
| Performance | Reuse dependencies (request cache) |
| Sync vs async | Prefer async dependencies |

## Implementation
1. Extract validation into dependency functions.
2. Chain dependencies for ownership/permission checks.
3. Reuse dependencies to leverage request caching.
4. Prefer `async def` dependencies unless truly sync.
5. See examples in `references/dependencies.md`.

## Example
```python
@router.get("/posts/{post_id}")
async def get_post(post: dict = Depends(valid_post_id)):
    return post
```

## Common Mistakes
- Duplicating validation in each endpoint
- Hiding complex logic in route handlers
- Using sync dependencies without need

## References
- `references/dependencies.md`
