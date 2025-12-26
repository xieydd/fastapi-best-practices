# Async Routes (FastAPI Best Practices)

## I/O Intensive Tasks
FastAPI can handle both async and sync I/O operations. Sync routes run in a thread pool, so blocking I/O does not block the event loop. Async routes should only do non-blocking I/O.

**Bad (blocking in async route):**
```python
import asyncio
import time

from fastapi import APIRouter

router = APIRouter()

@router.get("/terrible-ping")
async def terrible_ping():
    time.sleep(10)  # blocking I/O
    return {"pong": True}

@router.get("/good-ping")
def good_ping():
    time.sleep(10)  # sync route: blocking in a worker thread
    return {"pong": True}

@router.get("/perfect-ping")
async def perfect_ping():
    await asyncio.sleep(10)  # non-blocking
    return {"pong": True}
```

**Thread pool notes:**
- Threads cost more than coroutines.
- Thread pool size is limited; you can run out of threads and slow the app.

## CPU Intensive Tasks
- Awaiting CPU-heavy tasks does not help; the CPU still has to do the work.
- Running CPU-heavy work in threads is ineffective because of the GIL.
- Use another process for CPU-intensive work.

## Related References
- https://fastapi.tiangolo.com/async/#path-operation-functions
- https://en.wikipedia.org/wiki/Thread_pool
- https://docs.python.org/3/library/asyncio-eventloop.html
