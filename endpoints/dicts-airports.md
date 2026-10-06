# POST /api/ro/dicts/airports

Airports filtered by ids (all airports if no filter). Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/AirportController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | AirportRequest | yes | Ids filter (optional field) |

## Controller

```kotlin
@Cacheable("airports")
@PostMapping(AIRPORTS_PATH)
override fun getAll(@RequestBody request: AirportRequest): List<AirportDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/Airport.kt
data class AirportRequest(
    val ids: List<Int>? = null,
)
```

## Response

```kotlin
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

- `centroidWkt` is a WKT string in EPSG:4326.
