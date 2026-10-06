# GET /api/ro/farms/{farmId}/nearest-airport

Nearest airport to the farm: line geometry to it, distance in miles, airport city.

Source: `facade/src/main/kotlin/com/farmtogether/facade/reports/web/FarmNearestObjectController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/{farmId}/nearest-airport")
fun getFarmNearestAirport(@PathVariable farmId: Long): FarmNearestAirportUIModel =
    farmReportService.getFarmNearestAirport(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/common/model/FarmNearestObjectUIModels.kt
data class FarmNearestAirportUIModel(
    val farmId: Long,
    val lineToAirport: LineString? = null,
    val distanceInMiles: BigDecimal? = null,
    val city: String? = null,
)
```

## Notes

- `LineString` is a JTS type, serialized as GeoJSON.
