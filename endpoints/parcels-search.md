# POST /api/ro/parcels/search

Paginated search over parcels by ids, county, or APNs.

Source: `facade/src/main/kotlin/com/farmtogether/farms/app/parcels/web/ParcelController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | PageRequest&lt;ParcelFiltersDto&gt; | yes | Filters + pagination |

## Controller

```kotlin
@PostMapping(PARCEL_SEARCH_PATH)
override fun search(@RequestBody request: PageRequest<ParcelFiltersDto>): PageResponse<ParcelDto> =
    parcelFacade.search(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/farms/api/parcels/models/ParcelFiltersDto.kt
data class ParcelFiltersDto(
    val ids: List<Long>? = null,
    val countyId: Long? = null,
    val apns: List<String>? = null,
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

// facade-api/src/main/kotlin/com/farmtogether/farms/api/parcels/models/ParcelDto.kt
@Serializable
data class ParcelDto(
    val id: Long,
    val apn: String,
    val countyId: Long,
    val geometryWkt: String,
    val acres: BigDecimal,
    val address: String?,
    val owner: String?,
)
```
