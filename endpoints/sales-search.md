# POST /api/ro/sales/search

Paginated search over historical land sale records with filters (source, parcels, acres/price ranges, distance, crops, water districts, dates).

Source: `facade/src/main/kotlin/com/farmtogether/sales/app/web/SalesController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | PageRequest&lt;SaleFiltersDto&gt; | yes | Filters + pagination |

## Controller

```kotlin
@PostMapping(SALES_SEARCH_PATH)
override fun search(@RequestBody request: PageRequest<SaleFiltersDto>): PageResponse<SaleDto> =
    facade.search(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/sales/api/models/SaleFiltersDto.kt
data class SaleFiltersDto(
    val source: SaleDto.SourceDto? = null,
    val parcelIds: List<Long>? = null,
    val acresRange: Range<BigDecimal>? = null,
    val distance: DistanceFilter? = null,
    val cropIds: List<Long>? = null,
    val buildingsPercentLessThan: BigDecimal? = null,
    val tillablePercentGreaterThan: BigDecimal? = null,
    val pricePerTillableAcre: Range<BigDecimal>? = null,
    val date: Range<LocalDate>? = null,
    val hasAnyWaterDistrict: List<String>? = null,
) {
    data class Range<T>(
        val from: T? = null,
        val to: T? = null,
    )

    data class DistanceFilter(
        val wkt4326: String,
        val distanceInMeters: Int,
    )
}

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

// facade-api/src/main/kotlin/com/farmtogether/sales/api/models/SaleDto.kt
data class SaleDto(
    val id: Long,
    val source: SourceDto,
    val externalId: String,
    val acres: BigDecimal,
    val tillableAcres: BigDecimal,
    val price: BigDecimal,
    val pricePerTillable: BigDecimal,
    val date: LocalDate?,
    val centroidWkt4326: String?,
    val crops: List<CropDto>,
    val parcelIds: List<Long>,
    val apns: List<String>,
    val countyId: Long?,
    val buyers: List<String>,
    val sellers: List<String>,
    val waterDistricts: List<String>,
    val buildingsCount: Int,
    val buildingsAcres: BigDecimal,
    val note: String?,
) {
    data class CropDto(
        val id: Long?,
        val name: String,
    )

    enum class SourceDto {
        LISTING,
        ACREVALUE,
        APPRAISAL,
    }
}
```
