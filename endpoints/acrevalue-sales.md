# GET /api/ro/acrevalue/sales

Acrevalue sale records for a given county.

Source: `facade/src/main/kotlin/com/farmtogether/location/app/acrevalue/web/AcrevalueSalesController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| countyId | query | Long | yes | County identifier |

## Controller

```kotlin
@GetMapping(ACREVALUE_SALES_PATH)
override fun findSales(@RequestParam countyId: Long): List<AcrevalueSaleResponse> =
    acrevalueSalesService.findSaleRecords(countyId)
        .map { it.toResponse() }
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/location/api/models/AcrevalueSaleResponse.kt
data class AcrevalueSaleResponse(
    val id: String,
    val countyId: Long,
    val acres: BigDecimal,
    val crops: List<Crop>,
    val centroidWkt: String,
    val buyers: List<String>,
    val sellers: List<String>,
    val apns: List<String>,
    val saleDate: LocalDate?,
    val price: BigDecimal,
) {
    data class Crop(
        val clazz: String,
        val label: String,
        val percent: BigDecimal,
    )
}
```

## Notes

- `centroidWkt` is a WKT string.
