# GET /api/ro/farms/{farmId}/vegscape/report

Vegscape weekly vegetation (NDVI) report for a farm: status, weekly base64 images with coordinates.

Source: `facade/src/main/kotlin/com/farmtogether/facade/vegscape/VegscapeReportController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/farms/{farmId}/vegscape/report")
fun getReport(@PathVariable farmId: Long): VegscapeUIModels.Report =
    vegscapeReportService.getReport(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/vegscape/VegscapeUIModels.kt
object VegscapeUIModels {
    data class Week(
        val id: Long,
        val name: String,
        val averageNdvi: Double,
        val imageBase64: String,
        val imageCoordinates: ImageCoordinates?,
    )

    data class Report(
        val status: Status,
        val weeks: List<Week>,
        val imageCoordinates: ImageCoordinates? = null,
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
