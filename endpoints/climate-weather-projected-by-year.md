# POST /api/ro/climate/weather/projected/by-year

Projected weather aggregations (avg temperature, chilling hours) per year for a requested geometry.

Source: `facade/src/main/kotlin/com/farmtogether/climate/app/aggregations/common/web/WeatherAggregationsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | WeatherRequestDto | yes | Query geometry |

## Controller

```kotlin
@PostMapping(WEATHER_AGGREGATIONS_PROJECTED_GET_BY_YEAR_PATH)
override fun getProjectedByYear(@RequestBody request: WeatherRequestDto): ByYearAggregatedResponse =
    facade.getProjectedByYear(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/climate/api/models/WeatherRequests.kt
data class WeatherRequestDto(
    val geometry: String,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/climate/api/models/WeatherResponses.kt
data class ByYearAggregatedResponse(
    val years: List<ByYearAggregatedDto>
)

data class ByYearAggregatedDto(
    val year: Int,
    val averageTemperature: BigDecimal?,
    val chillingHours: Int?,
    val chillingHoursTrend: BigDecimal?,
)
```
