# POST /api/ro/arcgis/crops/search

Crop features for a given year intersecting a requested geometry (WKT), with crop attributes.

Source: `facade/src/main/kotlin/com/farmtogether/arcgis/app/crops/CropsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | CropsRequestDto | yes | Year + query geometry |

## Controller

```kotlin
@PostMapping(CROPS_SEARCH_PATH)
override fun search(@RequestBody request: CropsRequestDto): List<CropsFeatureDto> =
    service.search(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/CropsAttributesDto.kt
data class CropsRequestDto(
    val year: Int,
    val wkt4326: String,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/CropsAttributesDto.kt
data class CropsAttributesDto(
    val cropName: String,
    val usdaId: Int?,
)

typealias CropsFeatureDto = FeatureDto<CropsAttributesDto>

// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/FeatureDto.kt
data class FeatureDto<A>(
    val geometry: String,
    val attributes: A,
    val additionalAttributes: Map<String, Any?>,
)
```
