# POST /api/ro/dicts/counties

Counties filtered by ids/state/name/geometry/city search. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/CountyController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | CountyRequest | yes | Filters |

## Controller

```kotlin
@Cacheable("counties")
@PostMapping(COUNTY_PATH)
override fun getAll(@RequestBody request: CountyRequest): List<CountyDto> =
    service.getAll(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/County.kt
@Serializable
data class CountyRequest(
    val ids: List<Long>? = null,
    val stateId: Long? = null,
    val name: String? = null,
    val wkt4326: String? = null,
    val citySearch: CitySearchRequest? = null,
)

@Serializable
data class CitySearchRequest(
    val cityName: String,
    val stateShortName: String,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/County.kt
@Serializable
data class CountyDto(
    val id: Long,
    val name: String,
    val state: StateDto,
    val fips: String,
)

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/States.kt
@Serializable
data class StateDto(
    val id: Long,
    val name: String,
    val shortName: String,
)
```
