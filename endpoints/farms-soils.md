# GET /api/ro/farms/{farmId}/soils

Farm soil map as a GeoJSON FeatureCollection of soil polygons with soil properties.

Source: `facade/src/main/kotlin/com/farmtogether/facade/soils/web/FarmSoilController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/soils")
fun getFeatureCollection(@PathVariable farmId: Long): FeatureCollectionDto<SoilProperties> =
    soilService.getFeatureCollection(farmId)
```

## Response

```kotlin
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
```

## Notes

- `Geometry` is a JTS type, serialized as GeoJSON.
