# GET /api/ro/lidar/tree/tasks/list

All LiDAR tree tasks as a GeoJSON FeatureCollection (task properties per feature).

Source: `facade/src/main/kotlin/com/farmtogether/facade/lidar/LidarTreeTaskController.kt`

## Parameters

None.

## Controller

```kotlin
@GetMapping("/list")
fun getTasks(): FeatureCollectionDto<LidarTreeUIModels.TaskProperties> =
    lidarTreeTaskService.getTasks()
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
    data class TaskProperties(
        val id: String,
        val name: String,
        val status: String,
        val message: String?,
        val treeCount: Int?,
        val year: Int?,
        val created: String,
    )

    data class TreeProperties(
        val height: Double,
    )
}
```

## Notes

- `Geometry` is a JTS type, serialized as GeoJSON.
