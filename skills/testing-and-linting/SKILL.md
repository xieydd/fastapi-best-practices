---
name: testing-and-linting
description: Use when setting up FastAPI integration tests or enforcing formatting/linting standards with Ruff.
---

# Testing and Linting

## Overview
Start with async-first tests to avoid event loop issues and use Ruff for fast, unified linting.

## When to Use
- Building a new test suite for a FastAPI app
- Seeing event loop conflicts in integration tests
- Standardizing formatting and linting

## Quick Reference
| Topic | Guidance |
| --- | --- |
| Test client | Use async client from day 0 |
| Event loop | Mark tests with `@pytest.mark.asyncio` |
| Lint/format | Use Ruff for check + format |

## Implementation
1. Set up an async test client fixture.
2. Mark tests with `@pytest.mark.asyncio`.
3. Add Ruff scripts for lint/format.
4. See examples in `references/testing-and-linting.md`.

## Example
```python
@pytest.mark.asyncio
async def test_create_post(client: TestClient):
    resp = await client.post("/posts")
    assert resp.status_code == 201
```

## Common Mistakes
- Starting with sync tests and migrating later
- Mixing sync DB clients in async tests
- Skipping formatting/linting automation

## References
- `references/testing-and-linting.md`
