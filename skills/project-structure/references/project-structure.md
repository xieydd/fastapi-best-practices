# Project Structure (FastAPI Best Practices)

This section captures the recommended project layout and module conventions.

## Suggested Layout
```
fastapi-project
├── alembic/
├── src
│   ├── auth
│   │   ├── router.py
│   │   ├── schemas.py  # pydantic models
│   │   ├── models.py  # db models
│   │   ├── dependencies.py
│   │   ├── config.py  # local configs
│   │   ├── constants.py
│   │   ├── exceptions.py
│   │   ├── service.py
│   │   └── utils.py
│   ├── aws
│   │   ├── client.py  # client model for external service communication
│   │   ├── schemas.py
│   │   ├── config.py
│   │   ├── constants.py
│   │   ├── exceptions.py
│   │   └── utils.py
│   └── posts
│   │   ├── router.py
│   │   ├── schemas.py
│   │   ├── models.py
│   │   ├── dependencies.py
│   │   ├── constants.py
│   │   ├── exceptions.py
│   │   ├── service.py
│   │   └── utils.py
│   ├── config.py  # global configs
│   ├── models.py  # global models
│   ├── exceptions.py  # global exceptions
│   ├── pagination.py  # global module e.g. pagination
│   ├── database.py  # db connection related stuff
│   └── main.py
├── tests/
│   ├── auth
│   ├── aws
│   └── posts
├── templates/
│   └── index.html
├── requirements
│   ├── base.txt
│   ├── dev.txt
│   └── prod.txt
├── .env
├── .gitignore
├── logging.ini
└── alembic.ini
```

## Structure Principles
1. Store all domain directories inside `src`.
   - `src/` is the top-level of the app and contains common models/configs/constants.
   - `src/main.py` initializes the FastAPI app.
2. Each package owns its router, schemas, models, and support modules:
   - `router.py` - endpoints
   - `schemas.py` - Pydantic models
   - `models.py` - DB models
   - `service.py` - business logic
   - `dependencies.py` - router dependencies
   - `constants.py` - constants and error codes
   - `config.py` - local env/config
   - `utils.py` - non-business helpers
   - `exceptions.py` - module-specific exceptions
3. When importing between packages, use explicit module names:

```python
from src.auth import constants as auth_constants
from src.notifications import service as notification_service
from src.posts.constants import ErrorCode as PostsErrorCode
```
