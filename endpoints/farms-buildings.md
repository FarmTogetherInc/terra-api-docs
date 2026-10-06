# GET /api/ro/farms/{farmId}/buildings

Building footprints intersecting the farm as a GeoJSON FeatureCollection with building area (sq ft) properties.

Source: `facade/src/main/kotlin/com/farmtogether/facade/buildings/BuildingsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/buildings")
fun getBuildings(@PathVariable farmId: Long): BuildingsUIModel =
    buildingsService.getBuildings(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/buildings/BuildingsUIModel.kt
data class BuildingsUIModel(
    val type: String = "FeatureCollection",
    val features: List<Feature>,
) {
    data class Feature(
        val type: String = "Feature",
        val geometry: Geometry,
        val properties: Properties,
    )

    data class Properties(
        val areaSqFt: Int?,
    )
}
```

## Notes

- `Geometry` is a JTS type, serialized as GeoJSON.
