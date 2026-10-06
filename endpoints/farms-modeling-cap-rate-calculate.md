# POST /api/ro/farms/{farmId}/modeling/cap-rate/calculate

Calculates capitalization rate from submitted params and returns the rate plus an xlsx `documentId`. The `farmId` path variable is not used in the calculation.

Source: `facade/src/main/kotlin/com/farmtogether/facade/modeling/web/CapRateController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier (not used in calculation) |
| body | body | CapRateParamsUIModel | yes | Cap-rate model params |

## Controller

```kotlin
@PostMapping("/farms/{farmId}/modeling/cap-rate/calculate")
fun calculateCapRate(
    @PathVariable @Suppress("UNUSED_PARAMETER") farmId: Long,
    @RequestBody params: CapRateParamsUIModel
): CapRateResultUIModel =
    service.calculateCapRate(params)
```

## Request payload

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/modeling/model/CapRateUIModel.kt
data class CapRateParamsUIModel(
    val crop: String,
    val variety: String,
    val year: Int,
    val usedDefaultYear: Boolean,
    val acres: BigDecimal?,
)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/modeling/model/CapRateUIModel.kt
data class CapRateResultUIModel(
    val capRate: BigDecimal?,
    val documentId: String?,
)
```

## Notes

- Use `documentId` with [GET /documents/modeling/{documentId}](documents-modeling.md) to download the generated xlsx.
