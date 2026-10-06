# GET /api/ro/farms/{farmId}/report

Short farm report: farm id/name, deal name, geometry, parcel APNs/addresses.

Source: `facade/src/main/kotlin/com/farmtogether/facade/reports/web/FarmReportController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/report")
fun getReport(@PathVariable farmId: Long): FarmReportUIDto =
    service.getReport(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/reports/model/FarmReportUIModels.kt
data class FarmReportUIDto(
    val farmId: Long,
    val farmName: String,
    val dealName: String?,
    val geometry: Geometry?,
    val parcels: List<ParcelReportUIDto>,
)

data class ParcelReportUIDto(
    val parcelId: Long,
    val apn: String,
    val address: String?,
)
```

## Notes

- `Geometry` is a JTS type, serialized as GeoJSON.
