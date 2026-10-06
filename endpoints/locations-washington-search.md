# POST /api/ro/locations/washington/search

Paginated search over Washington State parcel data by APN, WKT geometry, or coordinates.

Source: `facade/src/main/kotlin/com/farmtogether/location/app/states/washington/web/WashingtonDataController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | PageRequest&lt;WashingtonDataFilters&gt; | yes | Filters + pagination |

## Controller

```kotlin
@PostMapping(WASHINGTON_SEARCH_PATH)
override fun search(@RequestBody request: PageRequest<WashingtonDataFilters>): PageResponse<WashingtonDto> =
    service.search(request)
        .map { it.toDto() }
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/location/api/models/WashingtonDataFilters.kt
data class WashingtonDataFilters(
    val apn: String? = null,
    val wkt4326: String? = null,
    val coordinates: Coordinates? = null,
)

// facade-api/src/main/kotlin/com/farmtogether/location/api/models/SearchRequest.kt
data class Coordinates(
    val lon: Double,
    val lat: Double
)

// utils/pagination-api/src/main/kotlin/com/farmtogether/common/page/PageModels.kt
@Serializable
data class PageRequest<R>(
    val filters: R? = null,
    val page: Pagination? = null,
    val sort: SortOrder? = null,
)

@Serializable
data class Pagination(
    val size: Int,
    val number: Int = 1,
)

@Serializable
data class SortOrder(
    val column: String,
    val direction: SortDirection = SortDirection.ASC,
)

enum class SortDirection {
    ASC,
    DESC,
}
```

## Response

```kotlin
// utils/pagination-api/src/main/kotlin/com/farmtogether/common/page/PageModels.kt
@Serializable
data class PageResponse<T>(
    val data: List<T>,
    val totalPages: Int,
    val totalElements: Long,
    val page: Pagination?,
) {
    fun <R> mapAll(f: (List<T>) -> List<R>): PageResponse<R> = PageResponse(
        data = f(data),
        totalPages,
        totalElements,
        page
    )

    fun <R> map(f: (T) -> (R)): PageResponse<R> = mapAll { data -> data.map(f) }

    companion object {
        fun <T> empty() = PageResponse<T>(
            data = emptyList(),
            totalPages = 0,
            totalElements = 0,
            page = null,
        )
    }
}

// facade-api/src/main/kotlin/com/farmtogether/location/api/models/WashingtonDto.kt
data class WashingtonDto(
    val id: Long,
    val wkbGeometry: String,
    val objectId: BigDecimal,
    val fipsNr: String,
    val countyNm: String,
    val parcelId: String,
    val origParcel: String?,
    val situsAddress: String?,
    val subAddress: String?,
    val situsCity: String?,
    val situsZip: String?,
    val landuseCd: BigDecimal?,
    val valueLand: BigDecimal?,
    val valueBldg: BigDecimal?,
    val dataLink: String?,
    val fileDate: LocalDate,
    val globalId: String,
)
```
