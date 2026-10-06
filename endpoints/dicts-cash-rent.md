# POST /api/ro/dicts/cash-rent

USDA cash rent statistics per year for a county. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/CashRentController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | CashRentRequest | yes | County filter |

## Controller

```kotlin
@Cacheable("cashRent")
@PostMapping(CASH_RENT_PATH)
override fun getAll(@RequestBody request: CashRentRequest): List<CashRentDto> =
    service.getAll(request)
        .map { it.toDto() }
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/CashRent.kt
data class CashRentRequest(
    val countyId: Long,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/CashRent.kt
data class CashRentDto(
    val year: Int,
    val croplandIrrigatedValue: BigDecimal?,
    val croplandIrrigatedCv: BigDecimal?,
    val croplandNonIrrigatedValue: BigDecimal?,
    val croplandNonIrrigatedCv: BigDecimal?,
    val pasturelandValue: BigDecimal?,
    val pasturelandCv: BigDecimal?,
)
```
