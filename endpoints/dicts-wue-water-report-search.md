# POST /api/ro/dicts/wue-water-report/search

WUE (water use efficiency) water report rows, optionally filtered by water district id.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/WueWaterReportController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | WueWaterReportRequest | yes | Water district filter (optional field) |

## Controller

```kotlin
@PostMapping(WUE_WATER_REPORT_PATH)
override fun getAll(@RequestBody request: WueWaterReportRequest): List<WueWaterReportDto> =
    service.findAll(request)
        .map { it.toDto() }
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WueWaterReportDto.kt
data class WueWaterReportRequest(
    val wdId: Long? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WueWaterReportDto.kt
data class WueWaterReportDto(
    val id: Long,
    val name: String,
    val year: Int,
    val acreage: BigDecimal,
    val annualDeliveries: BigDecimal,
    val url: String,
    val rawHtml: String,
)
```
