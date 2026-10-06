# POST /api/ro/dicts/buildings

Building footprint geometries (as WKT strings) intersecting the given WKT4326 geometry.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/BuildingFootprintController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | BuildingFootprintRequest | yes | Query geometry |

## Controller

```kotlin
@PostMapping(BUILDINGS_PATH)
override fun getAllGeometries(@RequestBody request: BuildingFootprintRequest): List<String> =
    service.getAllGeometries(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/BuildingFootprintRequest.kt
data class BuildingFootprintRequest(
    val wkt4326: String
)
```

## Response

`List<String>` — WKT geometries, no DTO.
