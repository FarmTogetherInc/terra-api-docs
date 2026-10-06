# POST /api/ro/dicts/dry-wells

Dry well reports near a geometry (within distance) reported after a date.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/DryWellsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | DryWellsRequest | yes | Filters |

## Controller

```kotlin
@PostMapping(DRY_WELLS_PATH)
override fun getAll(@RequestBody request: DryWellsRequest): List<DryWellDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/DryWell.kt
data class DryWellsRequest(
    val wkt4326: String,
    val distanceInMeters: Double,
    val reportDateAfter: LocalDate,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/DryWell.kt
data class DryWellDto(
    val id: Long,
    val createDate: LocalDate,
    val status: String,
    val shortageType: String?,
    val primaryUsages: String,
    val householdSupport: String?,
    val waterIssues: String?,
    val approximateIssueStartDate: LocalDate?,
    val county: String,
    val city: String?,
    val wellDepth: String?,
    val wasIssueResolved: String?,
    val approximateRepairCost: String?,
    val additionalInfo: String?,
    val wellToWaterDepth: String?,
    val measureDate: LocalDate?,
    val pumpRateReduction: String?,
    val reportDate: LocalDate,
    val statusType: String,
    val region: String,
    val wkt4326: String
)
```
