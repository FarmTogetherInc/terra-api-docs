# POST /api/ro/listings/price-changes

Listing price change events, optionally filtered by listing ids and an updated-date window.

Source: `facade/src/main/kotlin/com/farmtogether/listings/app/web/ListingsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | PriceChangesFilters | yes | Filters |

## Controller

```kotlin
@PostMapping(PRICE_CHANGES_PATH)
override fun getChanges(@RequestBody filters: PriceChangesFilters): List<ListingPriceChangeDto> =
    findService.findPriceChanges(filters)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/listings/api/models/ListingFilters.kt
data class PriceChangesFilters(
    val listingIds: List<Long>? = null,
    val updatedFrom: LocalDateTime? = null,
    val updatedTo: LocalDateTime? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/listings/api/models/ListingPriceChangeDto.kt
data class ListingPriceChangeDto(
    val listingId: Long,
    val url: String,
    val previousPrice: BigDecimal?,
    val price: BigDecimal,
    val date: LocalDate,
)
```
