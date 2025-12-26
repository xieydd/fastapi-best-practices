---
name: pydantic
description: Use when designing FastAPI request/response models, validation rules, shared base models, or environment settings with Pydantic.
---

# Pydantic

## Overview
Use Pydantic deeply for validation, transformation, and consistent model behavior across the app.

## When to Use
- Defining request/response schemas with constraints
- Standardizing serialization (e.g., datetime format)
- Splitting settings across domains instead of one global config

## Quick Reference
| Topic | Guidance |
| --- | --- |
| Validation | Prefer built-in types and validators |
| Base model | Create a custom base model for shared behavior |
| Settings | Split BaseSettings by domain |

## Implementation
1. Define strong schema constraints with `Field`, enums, and specialized types.
2. Create a custom base model for shared serialization behavior.
3. Split settings into domain configs (`src.auth.config`, `src.config`).
4. See full examples in `references/pydantic.md`.

## Example
```python
class UserBase(BaseModel):
    username: str = Field(min_length=1, max_length=128, pattern="^[A-Za-z0-9-_]+$")
    email: EmailStr
```

## Common Mistakes
- Manual validation when Pydantic types already cover it
- A single massive BaseSettings for the entire app
- Inconsistent datetime serialization across models

## References
- `references/pydantic.md`
