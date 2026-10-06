# POST /api/ro/dicts/hazards/hails

Paginated hail hazard records within a radius of a point from an optional date. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/HazardsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | PageRequest&lt;HailFilterDto&gt; | yes | Filters + pagination |

## Controller

```kotlin
@Cacheable("hazardsHails")
@PostMapping(HAILS_PATH)
override fun getAllHails(@RequestBody request: PageRequest<HailFilterDto>): PageResponse<HazardHailDto> =
    service.getAllHails(request)
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

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/HazardHail.kt
data class HailFilterDto(
    val wkt4326: String,
    val radiusMeters: Double,
    val dateFrom: LocalDate?,
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

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/HazardHail.kt
data class HazardHailDto(
    val id: Long,
    val wkt4326: String,
    val date: LocalDate,
    val sizeInches: BigDecimal,
    val lossText: String,
    val cropLossText: String,
)
```
