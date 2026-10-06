# POST /api/ro/locations/search

Search for a parcel/location by APN, county, coordinates, address, or owner (ReGrid/Washington/Fresno/California county sources), optionally using paid services.

Source: `facade/src/main/kotlin/com/farmtogether/location/app/common/web/SearchController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | SearchRequest | yes | Search criteria |

## Controller

```kotlin
@PostMapping(LOCATIONS_SEARCH_PATH)
override fun search(@RequestBody request: SearchRequest): SearchResponse =
    service.search(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/location/api/models/SearchRequest.kt
data class SearchRequest(
    val searchInStorageFirst: Boolean = true,
    val usePaidServices: Boolean? = null,
    val apn: String? = null,
    val stateId: Long? = null,
    val countyId: Long? = null,
    val coordinates: Coordinates? = null,
    val address: String? = null,
    val owner: String? = null,
)

data class Coordinates(
    val lon: Double,
    val lat: Double
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/location/api/models/SearchResponse.kt
data class SearchResponse(
    val status: ResponseStatus,
    val message: String? = null,
    val results: List<SearchResult> = emptyList()
)

data class SearchResult(
    val id: Long,
    val apn: String,
    val geom: String,
    val owner: String?,
    val address: String?,
    val landValue: BigDecimal?,
    val acres: BigDecimal?,
    val crops: List<Crop>,
    val source: ResultSource,
    val countyId: Long? = null,
)

data class Crop(
    val usdaId: Int,
    val percentage: BigDecimal,
    val date: String? = null,
)

// facade-api/src/main/kotlin/com/farmtogether/location/api/models/ResponseStatus.kt
enum class ResponseStatus {
    OK,
    EMPTY_RESULT,
    REGRID_SERVICE_ERROR,
    SERVER_ERROR
}

// facade-api/src/main/kotlin/com/farmtogether/location/api/models/ResultSource.kt
enum class ResultSource {
    REGRID,
    WASHINGTON_PARCELS,
    FRESNO,
    CALIFORNIA_ALAMEDA,
    CALIFORNIA_AMADOR,
    CALIFORNIA_BUTTE,
    CALIFORNIA_CALAVERAS,
    CALIFORNIA_COLUSA,
    CALIFORNIA_CONTRA_COSTA,
    CALIFORNIA_HUMBOLDT,
    CALIFORNIA_INYO,
    CALIFORNIA_KINGS,
    CALIFORNIA_MADERA,
    CALIFORNIA_MARIN,
    CALIFORNIA_MERCED,
    CALIFORNIA_MONO,
    CALIFORNIA_MONTEREY,
    CALIFORNIA_NAPA,
    CALIFORNIA_NEVADA,
    CALIFORNIA_PLACER,
    CALIFORNIA_RIVERSIDE,
    CALIFORNIA_SACRAMENTO,
    CALIFORNIA_SANTA_CLARA,
    CALIFORNIA_SANTA_CRUZ,
    CALIFORNIA_SAN_BENITO,
    CALIFORNIA_SAN_BERNARDINO,
    CALIFORNIA_SAN_DIEGO,
    CALIFORNIA_SAN_JOAQUIN,
    CALIFORNIA_SAN_LUIS_OBISPO,
    CALIFORNIA_SAN_MATEO,
    CALIFORNIA_SISKIYOU,
    CALIFORNIA_SONOMA,
    CALIFORNIA_STANISLAUS,
    CALIFORNIA_SUTTER,
    CALIFORNIA_TEHAMA,
    CALIFORNIA_YOLO,
    CALIFORNIA_YUBA,
    CALIFORNIA_STATEWIDE,
}
```

## Notes

- `geom` is a WKT string.
