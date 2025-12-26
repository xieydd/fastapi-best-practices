# Miscellaneous and Bonus (FastAPI Best Practices)

## Follow the REST
RESTful structure helps reuse dependencies across routes.

Example routes:
1. `GET /courses/:course_id`
2. `GET /courses/:course_id/chapters/:chapter_id/lessons`
3. `GET /chapters/:chapter_id`

Use consistent path variable names when dependencies chain.

```python
# src.profiles.dependencies
async def valid_profile_id(profile_id: UUID4) -> Mapping:
    profile = await service.get_by_id(profile_id)
    if not profile:
        raise ProfileNotFound()
    return profile

# src.creators.dependencies
async def valid_creator_id(profile: Mapping = Depends(valid_profile_id)) -> Mapping:
    if not profile["is_creator"]:
       raise ProfileNotCreator()
    return profile

# src.profiles.router.py
@router.get("/profiles/{profile_id}", response_model=ProfileResponse)
async def get_user_profile_by_id(profile: Mapping = Depends(valid_profile_id)):
    return profile

# src.creators.router.py
@router.get("/creators/{profile_id}", response_model=ProfileResponse)
async def get_user_profile_by_id(
     creator_profile: Mapping = Depends(valid_creator_id)
):
    return creator_profile
```

## Sync SDKs in Async Apps
If a third-party SDK is sync-only, run it in a threadpool.

```python
from fastapi import FastAPI
from fastapi.concurrency import run_in_threadpool
from my_sync_library import SyncAPIClient

app = FastAPI()

@app.get("/")
async def call_my_sync_library():
    my_data = await service.get_my_data()
    client = SyncAPIClient()
    await run_in_threadpool(client.make_request, data=my_data)
```

## Bonus Section
Community best practices are discussed in the project issues.
