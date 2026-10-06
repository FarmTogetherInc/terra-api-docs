# POST /api/ro/dicts/land-survey/by-geom

PLSS land survey sections intersecting the given WKT4326 geometry.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/LandSurveyController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | LandSurveyByCoordsRequest | yes | Query geometry |

## Controller

```kotlin
@PostMapping(SECTION_PATH)
override fun findSectionsByGeometry(@RequestBody request: LandSurveyByCoordsRequest): List<LandSurveyDto> =
    service.findSectionsByGeometry(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/LandSurvery.kt
data class LandSurveyByCoordsRequest(
    val wkt4326: String,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/LandSurvery.kt
data class LandSurveyDto(
    val id: Long,
    val wkt4326: String,
    val label: String,
)
```
