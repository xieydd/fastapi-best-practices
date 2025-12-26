---
name: async-routes
description: Use when deciding between async and sync FastAPI routes, diagnosing event loop blocking, or handling I/O vs CPU intensive work in request paths.
---

# Async Routes

## Overview
Choose the right execution model (async, sync, threadpool, or process) so FastAPI stays responsive under load.

## When to Use
- A route mixes blocking I/O and async code
- You need to decide whether a function should be `async def` or `def`
- You have CPU-bound work inside request handlers

## Quick Reference
| Scenario | Guidance |
| --- | --- |
| Blocking I/O | Use sync routes or threadpool |
| Non-blocking I/O | Use `async def` + `await` |
| CPU-bound work | Offload to separate process/worker |
| Sync SDK in async | Wrap with `run_in_threadpool` |

## Implementation
1. Identify whether the work is I/O-bound or CPU-bound.
2. Use `async def` only for non-blocking I/O.
3. Keep blocking I/O in sync routes or threadpool helpers.
4. Offload CPU-heavy work to a process or worker queue.
5. For detailed examples, read `references/async-routes.md`.

## Example
```python
from fastapi.concurrency import run_in_threadpool

@router.get("/sync-sdk")
async def call_sync_sdk():
    client = SyncAPIClient()
    await run_in_threadpool(client.make_request)
```

## Common Mistakes
- Calling `time.sleep()` inside `async def` routes
- Running CPU-heavy work in threads and expecting speedups
- Assuming `async` alone makes code parallel

## References
- `references/async-routes.md`
