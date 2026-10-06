# GET /api/ro/soils/tasks/{id}

Soil stats loading task status by id (null if unknown id).

Source: `facade/src/main/kotlin/com/farmtogether/facade/soils/web/SoilController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| id | path | String | yes | Soil loading task identifier |

## Controller

```kotlin
@GetMapping("/tasks/{id}")
fun status(@PathVariable id: String): SoilLoadingProgressUIModel? =
    soilService.checkStatus(id)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/soils/model/SoilModels.kt
data class SoilLoadingProgressUIModel(
    val waiting: Boolean,
    val finished: Boolean,
    val loadedPercent: Double,
)
```
