# POST /api/ro/dicts/cities

Paginated search of cities with optional distance/population filters. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/CityController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | PageRequest&lt;CityFilters&gt; | yes | Filters + pagination |

## Controller

```kotlin
@Cacheable("cities")
@PostMapping(CITIES_PATH)
override fun getAll(@RequestBody request: PageRequest<CityFilters>): PageResponse<CityDto> =
    service.getAll(request)
        .mapAll { it.toDtos() }
```

## Request payload

```kotlin
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

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/City.kt
data class CityFilters(
    val distance: CityDistanceFilter? = null,
    val populationGreaterThan: Int? = null,
)

data class CityDistanceFilter(
    val lessThanMeters: Double? = null,
    val wkt4326: String,
)
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
