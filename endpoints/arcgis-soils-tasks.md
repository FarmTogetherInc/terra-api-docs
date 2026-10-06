# GET /api/ro/arcgis/soils/tasks/{id}

Async soil-features loading task status by task id (null if unknown id).

Source: `facade/src/main/kotlin/com/farmtogether/arcgis/app/soils/SoilContractImpl.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| id | path | String | yes | Async loading task identifier |

## Controller

```kotlin
@GetMapping(SOILS_CHECK_STATUS_PATH)
override fun checkStatus(@PathVariable id: String): FeatureLoadingProgressDto? =
    service.getStatus(id)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/FeatureLoadingProgressDto.kt
data class FeatureLoadingProgressDto(
    val waiting: Boolean,
    val finished: Boolean,
    val loadedPercent: Double,
)
```
