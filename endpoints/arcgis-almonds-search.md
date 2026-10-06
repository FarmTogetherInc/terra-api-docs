# POST /api/ro/arcgis/almonds/search

Almond orchard features intersecting a requested geometry (WKT), with planting year per feature.

Source: `facade/src/main/kotlin/com/farmtogether/arcgis/app/almonds/web/AlmondController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | AlmondRequestDto | yes | Query geometry |

## Controller

```kotlin
@PostMapping(ALMONDS_SEARCH_PATH)
override fun search(@RequestBody request: AlmondRequestDto): List<AlmondDto> =
    service.searchAlmonds(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/AlmondDataDto.kt
data class AlmondRequestDto(
    val wkt4326: String,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/AlmondDataDto.kt
data class AlmondDto(
    val wkt4326: String,
    val yearPlanted: Int,
)
```
