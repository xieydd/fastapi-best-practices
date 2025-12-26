# Testing and Linting (FastAPI Best Practices)

## Async Test Client from Day 0
Use async test client early to avoid event loop issues later.

```python
import pytest
from async_asgi_testclient import TestClient

from src.main import app

@pytest.fixture
async def client() -> AsyncGenerator[TestClient, None]:
    host, port = "127.0.0.1", "9000"

    async with AsyncClient(transport=ASGITransport(app=app, client=(host, port)), base_url="http://test") as client:
        yield client

@pytest.mark.asyncio
async def test_create_post(client: TestClient):
    resp = await client.post("/posts")
    assert resp.status_code == 201
```

## Use Ruff
Ruff can replace black, autoflake, isort, and provides many lint rules.

```sh
#!/bin/sh -e
set -x

ruff check --fix src
ruff format src
```
