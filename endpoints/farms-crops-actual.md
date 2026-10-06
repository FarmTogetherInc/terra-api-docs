# GET /api/ro/farms/{farmId}/crops/actual

User-entered (actual) crops for a farm.

Source: `facade/src/main/kotlin/com/farmtogether/facade/crops/web/FarmActualCropsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/farms/{farmId}/crops/actual")
fun getActualCrops(
    @PathVariable farmId: Long,
): FarmActualCropsListUIModel = farmActualCropsService.getActualCrops(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/crops/model/FarmActualCropsUIModel.kt
data class FarmActualCropsUIModel(
    val cropId: Long,
    val cropName: String?,
    val color: String?,
    val varietyId: Long?,
    val varietyName: String?,

    @get:Positive
    val acreage: BigDecimal?,

    @get:PastOrPresent
    val yearPlanted: Year?,
)

data class FarmActualCropsListUIModel(
    @get:Valid
    val crops: List<FarmActualCropsUIModel>,
)
```
