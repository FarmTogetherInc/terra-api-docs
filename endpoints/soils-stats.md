# POST /api/ro/soils/stats

Soil statistics for a requested area as a GeoJSON FeatureCollection of soil properties.

Source: `facade/src/main/kotlin/com/farmtogether/facade/soils/web/SoilController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | SoilRequestUIModel | yes | Query geometry |

## Controller

```kotlin
@PostMapping("/stats")
fun stats(@RequestBody request: SoilRequestUIModel): FeatureCollectionDto<SoilProperties> =
    soilService.getFeatureCollection(request.geometry)
```

## Request payload

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/soils/model/SoilModels.kt
data class SoilRequestUIModel(
    val geometry: Geometry,
)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/common/model/FeatureCollectionDto.kt
data class FeatureCollectionDto<P>(
    val features: List<FeatureDto<P>>
) {
    val type = "FeatureCollection"
}

data class FeatureDto<P>(
    val properties: P,
    val geometry: Geometry,
) {
    val type = "Feature"
}

// facade/src/main/kotlin/com/farmtogether/facade/soils/model/SoilModels.kt
data class SoilProperties(
    val mapUnitSymbol: String,
    val color: String,
    val type: String,
    val slopesPercent: Int?,
    val irrigatedClass: Int?,
    val acres: BigDecimal?,
    val percentOfTotal: BigDecimal?,
    val additionalProperties: Map<String, Any?>,
)
```

## Notes

- `Geometry` is a JTS type, serialized as GeoJSON.
