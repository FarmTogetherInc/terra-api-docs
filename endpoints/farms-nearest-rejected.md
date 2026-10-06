# POST /api/ro/farms/{farmId}/nearest-rejected

Recently rejected (passed-on) farms within a distance/age window around the given farm.

Source: `facade/src/main/kotlin/com/farmtogether/facade/reports/web/FarmNearestObjectController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |
| body | body | RejectedFarmUIRequest | yes | Search window |

## Controller

```kotlin
@PostMapping("/{farmId}/nearest-rejected")
fun getNearestRejected(
    @PathVariable farmId: Long,
    @RequestBody request: RejectedFarmUIRequest,
): List<RejectedFarmUIModel> =
    rejectedFarmReportService.getNearestRejected(farmId, request)
```

## Request payload

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/reports/model/RejectedFarmUIModel.kt
data class RejectedFarmUIRequest(
    val distanceLessThanMiles: Int,
    val maxAgeInDays: Int,
)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/reports/model/RejectedFarmUIModel.kt
data class RejectedFarmUIModel(
    val id: Long,
    val centroid: Point?,
    val name: String,
    val passReason: String?,
    val daysAgo: Int?,
    val rejectedBy: String?,
)
```

## Notes

- `Point` is a JTS type, serialized as GeoJSON.
