# GET /api/ro/lidar/tree/tasks/{id}

A single LiDAR tree task as a GeoJSON Feature (null if not found).

Source: `facade/src/main/kotlin/com/farmtogether/facade/lidar/LidarTreeTaskController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| id | path | String | yes | Task identifier |

## Controller

```kotlin
@GetMapping("/{id}")
fun getTask(@PathVariable id: String): FeatureDto<LidarTreeUIModels.TaskProperties>? =
    lidarTreeTaskService.getTask(id)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/common/model/FeatureCollectionDto.kt
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
}
```

## Notes

- `Geometry` is a JTS type, serialized as GeoJSON.
