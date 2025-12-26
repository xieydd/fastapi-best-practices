---
name: docs-and-serialization
description: Use when shaping FastAPI responses, configuring OpenAPI documentation, or exposing validation errors and response models safely.
---

# Docs and Serialization

## Overview
Avoid double serialization costs, expose clean validation errors, and keep API docs explicit and environment-safe.

## When to Use
- You are returning Pydantic instances from routes
- You need to hide docs in prod or enrich OpenAPI output
- You want validation errors to be user-friendly

## Quick Reference
| Topic | Guidance |
| --- | --- |
| Response models | Returning instances does not skip validation |
| Validation errors | Raise `ValueError` in validators |
| Docs exposure | Disable docs unless env is allowlisted |
| Docs quality | Use response_model, status_code, responses |

## Implementation
1. Return dicts or domain data; let response_model do validation.
2. Use Pydantic validators for user-facing errors.
3. Guard docs with env allowlist.
4. Add rich response metadata on routes.
5. See examples in `references/docs-and-serialization.md`.

## Example
```python
app_configs = {"title": "My Cool API"}
if ENVIRONMENT not in ("local", "staging"):
    app_configs["openapi_url"] = None
```

## Common Mistakes
- Assuming returning a Pydantic instance is faster
- Exposing docs in production by default
- Missing response_model and responses metadata

## References
- `references/docs-and-serialization.md`
