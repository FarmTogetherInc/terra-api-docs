# POST /api/ro/listings/list

Paginated listing search (by site, ids, url, tracking flag); each listing is returned with its latest snapshot data.

Source: `facade/src/main/kotlin/com/farmtogether/listings/app/web/ListingsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | PageRequest&lt;ListingFilters&gt; | yes | Filters + pagination |

## Controller

```kotlin
@PostMapping(GET_LISTINGS_PATH)
override fun getListings(@RequestBody request: PageRequest<ListingFilters>): PageResponse<ListingDto> =
    findService.findWithSnapshots(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/listings/api/models/ListingFilters.kt
data class ListingFilters(
    val site: String? = null,
    val ids: List<Long>? = null,
    val url: String? = null,
    val trackingEnabled: Boolean? = null,
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

// facade-api/src/main/kotlin/com/farmtogether/listings/api/models/ListingDto.kt
data class ListingDto(
    val id: Long,
    val created: LocalDateTime,
    val updated: LocalDateTime?,
    val lastSeen: LocalDateTime,
    val acreage: BigDecimal,
    val address: String?,
    val apns: List<String>,
    val centroidWkt: String?,
    val comment: String?,
    val countyId: Long?,
    val crops: List<ListingCrop>,
    val description: String?,
    val documents: List<ListingDocument>,
    val name: String,
    val geometryWkt: String?,
    val plantings: String?,
    val price: BigDecimal,
    val siteName: String,
    val status: MarketStatus,
    val url: String,
    val waterDistrictIds: List<Long>,
    val waterText: String?,
)

// facade-api/src/main/kotlin/com/farmtogether/listings/api/models/ListingCrop.kt
data class ListingCrop(
    val cropId: Long,
    val varietyId: Long? = null,
    val acreage: BigDecimal? = null,
    val yearPlanted: Int? = null,
) : Comparable<ListingCrop> {
    override fun compareTo(other: ListingCrop): Int {
        return cropId.compareTo(other.cropId)
    }
}

// facade-api/src/main/kotlin/com/farmtogether/listings/api/models/ListingDocument.kt
data class ListingDocument(
    val url: String,
    val name: String,
) : Comparable<ListingDocument> {
    override fun compareTo(other: ListingDocument): Int {
        return name.compareTo(other.name)
    }
}

// facade-api/src/main/kotlin/com/farmtogether/listings/api/models/MarketStatus.kt
enum class MarketStatus {
    ACTIVE,
    PENDING,
    SOLD,
    UNKNOWN,
}
```

## Notes

- `centroidWkt`/`geometryWkt` are WKT strings.
