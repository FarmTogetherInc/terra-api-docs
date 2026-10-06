# GET /api/ro/lidar/tree/tasks/{id}/data

Detected trees for a task as a GeoJSON FeatureCollection (tree height per feature).

Source: `facade/src/main/kotlin/com/farmtogether/facade/lidar/LidarTreeTaskController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| id | path | String | yes | Task identifier |

## Controller

```kotlin
@GetMapping("/{id}/data")
fun getTrees(@PathVariable id: String): FeatureCollectionDto<LidarTreeUIModels.TreeProperties> =
    lidarTreeTaskService.getTrees(id)
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

// facade/src/main/kotlin/com/farmtogether/facade/lidar/LidarTreeUIModels.kt
object LidarTreeUIModels {
    data class TreeProperties(
        val height: Double,
    )
}
```

## Notes

- `Geometry` is a JTS type, serialized as GeoJSON.
