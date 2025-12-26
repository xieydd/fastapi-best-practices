---
name: misc-bonus
description: Use when designing RESTful FastAPI routes, integrating sync-only SDKs into async apps, or looking for community best-practice add-ons.
---

# Miscellaneous and Bonus

## Overview
Keep routing RESTful, integrate sync SDKs safely, and reference community best-practice extensions.

## When to Use
- Designing nested REST endpoints and shared dependencies
- Calling sync-only SDKs inside async routes
- Looking for additional best practices from community issues

## Quick Reference
| Topic | Guidance |
| --- | --- |
| REST paths | Keep path params consistent for dependency reuse |
| Sync SDKs | Use `run_in_threadpool` |
| Bonus ideas | Check project issues for extra patterns |

## Implementation
1. Normalize route parameter names to enable dependency chaining.
2. Wrap sync SDK calls in `run_in_threadpool`.
3. See examples and notes in `references/misc-bonus.md`.

## Example
```python
await run_in_threadpool(client.make_request, data=my_data)
```

## Common Mistakes
- Inconsistent path params that break dependency reuse
- Calling sync SDKs directly in `async def` routes

## References
- `references/misc-bonus.md`
