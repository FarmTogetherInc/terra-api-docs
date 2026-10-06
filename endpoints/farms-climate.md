# GET /api/ro/farms/{farmId}/climate

Climate/chill-hours data for the farm: crops with chilling-hour ranges, summary, historical and projected chart values.

Source: `facade/src/main/kotlin/com/farmtogether/facade/climate/web/FarmClimateController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/climate")
fun getClimateData(@PathVariable farmId: Long): FarmClimateUIModels.Response =
    farmClimateService.getClimateData(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/climate/model/FarmClimateUIModels.kt
object FarmClimateUIModels {
    data class Response(
        val crops: List<Crop>,
        val summary: CalculatedChillHours,
        val historical: List<ChartValue>,
        val projected: List<ChartValue>,
    )

    data class CalculatedChillHours(
        val recentHistorical: Int?,
        val lastProjected: Int?,
    )

    data class Crop(
        val color: String,
        val name: String,
        val chillingHoursRange: ChillingHoursRange?,
    )

    data class ChillingHoursRange(
        val min: Int,
        val max: Int,
    )

    data class ChartValue(
        val year: Int,
        val averageTemperature: BigDecimal?,
        val chillingHours: Int?,
        val chillingHoursTrend: BigDecimal?,
    )
}
```
