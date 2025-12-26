# Database, Migrations, Naming (FastAPI Best Practices)

## Set DB Keys Naming Conventions
Explicit index naming is preferable to SQLAlchemy defaults.

```python
from sqlalchemy import MetaData

POSTGRES_INDEXES_NAMING_CONVENTION = {
    "ix": "%(column_0_label)s_idx",
    "uq": "%(table_name)s_%(column_0_name)s_key",
    "ck": "%(table_name)s_%(constraint_name)s_check",
    "fk": "%(table_name)s_%(column_0_name)s_fkey",
    "pk": "%(table_name)s_pkey",
}
metadata = MetaData(naming_convention=POSTGRES_INDEXES_NAMING_CONVENTION)
```

## Migrations (Alembic)
1. Migrations must be static and reversible. If data is dynamic, structure should still be static.
2. Use descriptive names & slugs.
3. Set human-readable file template, e.g. `2022-08-24_post_content_idx.py`.

```ini
# alembic.ini
file_template = %%(year)d-%%(month).2d-%%(day).2d_%%(slug)s
```

## Set DB Naming Conventions
1. lower_case_snake
2. singular form (e.g. `post`, `post_like`)
3. group similar tables with module prefix (e.g. `payment_account`)
4. consistent naming across tables (use specific names when needed)
5. `_at` suffix for datetime
6. `_date` suffix for date

## SQL-first, Pydantic-second
Prefer complex joins and aggregation in SQL, and keep Pydantic for modeling the result.

```python
# src.posts.service
from typing import Any

from pydantic import UUID4
from sqlalchemy import desc, func, select, text
from sqlalchemy.sql.functions import coalesce

from src.database import database, posts, profiles

async def get_posts(
    creator_id: UUID4, *, limit: int = 10, offset: int = 0
) -> list[dict[str, Any]]:
    select_query = (
        select(
            (
                posts.c.id,
                posts.c.slug,
                posts.c.title,
                func.json_build_object(
                   text("'id', profiles.id"),
                   text("'first_name', profiles.first_name"),
                   text("'last_name', profiles.last_name"),
                   text("'username', profiles.username"),
                ).label("creator"),
            )
        )
        .select_from(posts.join(profiles, posts.c.owner_id == profiles.c.id))
        .where(posts.c.owner_id == creator_id)
        .limit(limit)
        .offset(offset)
        .group_by(
            posts.c.id,
            posts.c.type,
            posts.c.slug,
            posts.c.title,
            profiles.c.id,
            profiles.c.first_name,
            profiles.c.last_name,
            profiles.c.username,
            profiles.c.avatar,
        )
        .order_by(
            desc(coalesce(posts.c.updated_at, posts.c.published_at, posts.c.created_at))
        )
    )
    return await database.fetch_all(select_query)

# src.posts.schemas
from pydantic import BaseModel, UUID4

class Creator(BaseModel):
    id: UUID4
    first_name: str
    last_name: str
    username: str

class Post(BaseModel):
    id: UUID4
    slug: str
    title: str
    creator: Creator
```
