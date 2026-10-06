# GET /api/ro/farms/{farmId}/sentinel/report

Sentinel satellite NDVI/NDMI report for a farm: status plus per-item image URLs, boundaries and averages.

Source: `facade/src/main/kotlin/com/farmtogether/facade/sentinel/SentinelReportController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/farms/{farmId}/sentinel/report")
fun getReport(@PathVariable farmId: Long): SentinelUIModels.Report =
    sentinelReportService.getReport(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/sentinel/SentinelUIModels.kt
object SentinelUIModels {
    data class Item(
        val id: Long,
        val name: String,
        val cloudRatio: Double,
        val averageNdmi: Double,
        val averageNdvi: Double,

        val ndmiPictureUrl: String,
        val ndmiBoundaries: ImageCoordinates,

        val ndviPictureUrl: String,
        val ndviBoundaries: ImageCoordinates,

        val overviewPictureUrl: String,
        val overviewBoundaries: ImageCoordinates,
    )

    data class Report(
        val status: Status,
        val items: List<Item>,
    )

    enum class Status {
        NO_DATA,
        IN_PROGRESS,
        COMPLETED,
    }
}

// tiff-service/tiff-service-api/src/main/kotlin/com/farmtogether/tiffs/api/models/VegscapeDtos.kt
typealias ImageCoordinates = List<List<Double>>
```
