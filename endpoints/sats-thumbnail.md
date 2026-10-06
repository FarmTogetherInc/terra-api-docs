# GET /sats/thumbnail

Satellite scene thumbnail/preview image, served **exclusively from the S3 cache**.
No paid provider call is possible on this path. The cache is filled write-through
by internal usage only — if a scene has never been fetched internally, the
thumbnail is absent and this endpoint returns `404` (cold cache; do not retry).

## Query parameters

| Name | Type | Notes |
|---|---|---|
| `provider` | enum `PLANET` \| `SKYWATCH` | |
| `scene_id` | string | provider scene id (e.g. `skyWatchId` / Planet `sceneId` from metadata searches) |

## Response

- `200` — raw image bytes, `Content-Type` from the cached provider response
- `404` — thumbnail not in cache

Source of truth: `facade/src/main/kotlin/com/farmtogether/satellites/app/common/web/SatsThumbnailController.kt`, cache in `satellites/app/common/service/SatsThumbnailCacheService.kt` (S3 key `sats/thumbnails/{provider}/{sceneId}`).
