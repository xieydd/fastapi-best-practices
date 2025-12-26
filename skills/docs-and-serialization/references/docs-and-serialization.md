# Docs and Serialization (FastAPI Best Practices)

## FastAPI Response Serialization
Returning a Pydantic model instance does not avoid validation. FastAPI will:
1) Convert it to dict (jsonable_encoder)
2) Validate with response_model
3) Serialize to JSON

That means the Pydantic model gets created twice if you return an instance.

```python
from fastapi import FastAPI
from pydantic import BaseModel, model_validator

app = FastAPI()

class ProfileResponse(BaseModel):
    @model_validator(mode="after")
    def debug_usage(self):
        print("created pydantic model")
        return self

@app.get("/", response_model=ProfileResponse)
async def root():
    return ProfileResponse()
```

## ValueError -> ValidationError
Raising ValueError inside Pydantic validators surfaces a helpful ValidationError to clients.

```python
from pydantic import BaseModel, field_validator

class ProfileCreate(BaseModel):
    username: str

    @field_validator("password", mode="after")
    @classmethod
    def valid_password(cls, password: str) -> str:
        if not re.match(STRONG_PASSWORD_PATTERN, password):
            raise ValueError(
                "Password must contain at least one lower character, "
                "one upper character, digit or special symbol"
            )
        return password
```

## Docs Hygiene
Hide docs by default unless environment explicitly allows them.

```python
from fastapi import FastAPI
from starlette.config import Config

config = Config(".env")
ENVIRONMENT = config("ENVIRONMENT")
SHOW_DOCS_ENVIRONMENT = ("local", "staging")

app_configs = {"title": "My Cool API"}
if ENVIRONMENT not in SHOW_DOCS_ENVIRONMENT:
   app_configs["openapi_url"] = None

app = FastAPI(**app_configs)
```

## Better OpenAPI Docs
Provide rich docs using response_model, status_code, description, tags, summary, and responses.

```python
from fastapi import APIRouter, status

router = APIRouter()

@router.post(
    "/endpoints",
    response_model=DefaultResponseModel,
    status_code=status.HTTP_201_CREATED,
    description="Description of the well documented endpoint",
    tags=["Endpoint Category"],
    summary="Summary of the Endpoint",
    responses={
        status.HTTP_200_OK: {
            "model": OkResponse,
            "description": "Ok Response",
        },
        status.HTTP_201_CREATED: {
            "model": CreatedResponse,
            "description": "Creates something from user request",
        },
        status.HTTP_202_ACCEPTED: {
            "model": AcceptedResponse,
            "description": "Accepts request and handles it later",
        },
    },
)
async def documented_route():
    pass
```
