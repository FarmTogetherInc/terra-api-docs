# GET /api/ro/farms/{farmId}/topography

Farm topography as a GeoJSON FeatureCollection of elevation zones plus min/max/avg elevation stats; null when nothing computed yet.

Source: `facade/src/main/kotlin/com/farmtogether/facade/elevation/ElevationController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/farms/{farmId}/topography")
fun getTopography(@PathVariable farmId: Long): TopographyUIResponse? =
    service.getTopography(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/elevation/ElevationUIModels.kt
data class TopographyUIResponse(
    val geojson: FeatureCollectionDto<TopographyProperties>,
    val stats: TopographyStatsUIDto,
)

data class TopographyStatsUIDto(
    val minElevationFeet: Int,
    val maxElevationFeet: Int,
    val avgElevationFeet: Int,
)

data class TopographyProperties(
    val areaAcres: Double,
    val elevationFeet: Int,
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
