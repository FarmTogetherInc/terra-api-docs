# POST /api/ro/dicts/cities/closest

Single closest city to a point (WKT4326) with population above a threshold.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/CityController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | ClosestCityRequest | yes | Query point + population filter |

## Controller

```kotlin
@PostMapping(CLOSEST_CITY_PATH)
override fun findClosest(@RequestBody request: ClosestCityRequest): CityDto? =
    service.findClosestCity(request)?.toDto()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/City.kt
data class ClosestCityRequest(
    val wkt4326: String,
    val populationGreaterThan: Int,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/City.kt
data class CityDto(
    val id: Long,
    val wkt4326: String,
    val gnisId: BigDecimal,
    val ansicode: String,
    val feature: String,
    val feature2: String,
    val name: String,
    val populationIn2010: BigDecimal,
    val county: String,
    val countyFips: String,
    val state: String,
    val stateFips: String,
    val latitude: Double,
    val longitude: Double,
    val poppllat: Double,
    val poppllong: Double,
    val elevationInMeters: BigDecimal,
    val elevationInFts: BigDecimal,
)
```
