# GET /api/ro/farms/{id}/lidar

Properties of the LiDAR tree-analysis task attached to the farm (null if the farm has no `lidarTreeTaskId`). Note the path placeholder is `{id}`.

Source: `facade/src/main/kotlin/com/farmtogether/facade/lidar/FarmLidarTaskController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| id | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping
fun getFarmTask(@PathVariable id: Long): LidarTreeUIModels.TaskProperties? =
    farmService.getFarmDto(id).lidarTreeTaskId.nullIfBlank().let { lidarTreeTaskId ->
        if(lidarTreeTaskId == null) {
            logger.warn("Farm($id).lidarTreeTaskId is NULL")
            null
        } else {
            lidarTreeTaskService.getTask(lidarTreeTaskId)?.properties
        }
    }
```

## Response

```kotlin
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

    data class TaskCreateRequest(
        val geometry: Geometry,
    )

    data class TreeProperties(
        val height: Double,
    )
}
```

## Notes

- `TaskCreateRequest` is shown for context only; task creation (POST) is not exposed in the read-only API.
