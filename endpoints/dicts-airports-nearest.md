# POST /api/ro/dicts/airports/nearest

Nearest airport to a given centroid (WKT 4326) with connecting linestring and distance.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/AirportController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | AirportNearestRequest | yes | Query point |

## Controller

```kotlin
@Cacheable("airports-nearest")
@PostMapping(NEAREST_AIRPORT_PATH)
override fun getNearest(@RequestBody request: AirportNearestRequest): AirportNearestResponse? =
    service.getNearest(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/AirportNearest.kt
data class AirportNearestRequest(
    val centroid4326wkt: String
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/AirportNearest.kt
data class AirportNearestResponse(
    val airport: AirportDto,
    val linestringWkt: String,
    val distanceInMiles: BigDecimal,
)

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/Airport.kt
data class AirportDto(
    val id: Int,
    val icaoCode: String,
    val iataCode: String,
    val name: String,
    val city: String,
    val country: String,
    val altitude: Int,
    val centroidWkt: String
)
```

## Notes

- All geometry fields are WKT strings in EPSG:4326.
