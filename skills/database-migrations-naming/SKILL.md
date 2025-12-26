---
name: database-migrations-naming
description: Use when defining database naming conventions, configuring Alembic migrations, or deciding where SQL vs Pydantic should handle data shaping.
---

# Database, Migrations, Naming

## Overview
Standardize DB naming, write reliable migrations, and push data shaping into SQL where it performs best.

## When to Use
- Setting table/index/constraint naming conventions
- Creating or reviewing Alembic migrations
- Designing queries that feed API responses

## Quick Reference
| Topic | Guidance |
| --- | --- |
| Naming | snake_case, singular, consistent prefixes |
| Migrations | Static, reversible, descriptive slug |
| Indexes | Set SQLAlchemy naming conventions |
| Data shaping | Prefer SQL joins/aggregation |

## Implementation
1. Define naming conventions in metadata.
2. Configure Alembic `file_template` with date + slug.
3. Enforce naming rules across tables and columns.
4. Push heavy joins/aggregation into SQL queries.
5. See full details in `references/database-migrations-naming.md`.

## Example
```python
metadata = MetaData(naming_convention={
    "ix": "%(column_0_label)s_idx",
    "pk": "%(table_name)s_pkey",
})
```

## Common Mistakes
- Dynamic migrations that cannot be reversed
- Inconsistent table naming across modules
- JSON aggregation done in Python instead of SQL

## References
- `references/database-migrations-naming.md`
