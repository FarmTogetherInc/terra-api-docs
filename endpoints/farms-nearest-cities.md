# GET /api/ro/farms/{farmId}/nearest-cities

Cities nearest to the farm, optionally filtered by max distance and min population.

Source: `facade/src/main/kotlin/com/farmtogether/facade/reports/web/FarmNearestObjectController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |
| distance-less-than-miles | query | Int | no | Keep only cities closer than this many miles |
| population-greater-than | query | Int | no | Keep only cities with population greater than this |

## Controller

```kotlin
@GetMapping("/{farmId}/nearest-cities")
fun getNearestCities(
    @PathVariable farmId: Long,
    @RequestParam("distance-less-than-miles", required = false) distanceLessThanMiles: Int?,
    @RequestParam("population-greater-than", required = false) populationGreaterThan: Int?,
): List<FarmNearestCityUIModel> =
    farmReportService.getNearestCities(
        farmId = farmId,
        distanceLessThanMiles = distanceLessThanMiles,
        populationGreaterThan = populationGreaterThan,
    )
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/common/model/FarmNearestObjectUIModels.kt
data class FarmNearestCityUIModel(
    val cityId: Long,
    val farmId: Long,
    val distanceInMiles: BigDecimal? = null,
    val cityName: String,
    val populationIn2010: Long,
    val cityCentroid: Point,
)
```

## Notes

- `Point` is a JTS type, serialized as GeoJSON.
