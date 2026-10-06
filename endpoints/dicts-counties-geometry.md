# GET /api/ro/dicts/counties/{id}/geometry

County boundary geometry as a WKT string in EPSG:4326. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/CountyController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| id | path | Long | yes | County identifier |

## Controller

```kotlin
@Cacheable("counties-wkt")
@GetMapping(COUNTY_GEOMETRY_PATH)
override fun getWkt4326(@PathVariable id: Long): String? =
    service.getById(id)?.geometry?.toText()
```

## Response

Plain `String?` (WKT, EPSG:4326) — no DTO.
