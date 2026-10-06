# GET /api/ro/farms/{farmId}/price-history

Farm price history entries (price, date, whether captured by scrapper).

Source: `facade/src/main/kotlin/com/farmtogether/facade/overview/web/FarmOverviewController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/{farmId}/price-history")
fun priceHistory(@PathVariable farmId: Long): List<FarmPriceUIModel> =
    farmPriceHistoryService.getPriceHistory(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/overview/model/OverviewUIModels.kt
data class FarmPriceUIModel(
    val price: BigDecimal,
    val date: LocalDate,
    val isScrapper: Boolean,
)
```
