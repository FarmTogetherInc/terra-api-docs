# POST /api/ro/climate/weather/historical/by-year

Historical weather aggregations (avg temperature, chilling hours) per year for a requested geometry.

Source: `facade/src/main/kotlin/com/farmtogether/climate/app/aggregations/common/web/WeatherAggregationsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | WeatherRequestDto | yes | Query geometry |

## Controller

```kotlin
@PostMapping(WEATHER_AGGREGATIONS_HISTORICAL_GET_BY_YEAR_PATH)
override fun getHistoricalByYear(@RequestBody request: WeatherRequestDto): ByYearAggregatedResponse =
    facade.getHistoricalByYear(request)
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
