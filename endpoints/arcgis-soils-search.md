# POST /api/ro/arcgis/soils/search

Soil map-unit features intersecting a requested geometry (WKT), with soil attributes.

Source: `facade/src/main/kotlin/com/farmtogether/arcgis/app/soils/SoilContractImpl.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | FeatureRequestDto | yes | Query geometry |

## Controller

```kotlin
@PostMapping(SOILS_SEARCH_PATH)
override fun search(@RequestBody request: FeatureRequestDto): List<SoilFeatureDto> =
    service.search(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/FeatureRequestDto.kt
data class FeatureRequestDto(
    val geometry: String, // WKT with SRID 4326
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/SoilAttributesDto.kt
data class SoilAttributesDto(
    val iccdc: Int?,
    val niccdc: Int?,
    val mapUnitName: String,
    val mapUnitKey: String,
    val slopeGradientDominantComponent: Int?,
    val color: String,
    val mapUnitSymbol: String,
)

typealias SoilFeatureDto = FeatureDto<SoilAttributesDto>

// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/FeatureDto.kt
data class FeatureDto<A>(
    val geometry: String,
    val attributes: A,
    val additionalAttributes: Map<String, Any?>,
)
```
