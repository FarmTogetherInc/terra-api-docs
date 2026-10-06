# GET /api/ro/farms/{farmId}/modeling/cap-rate/params

Cap-rate modeling parameter presets for a farm (crop/variety/year/acreage rows).

Source: `facade/src/main/kotlin/com/farmtogether/facade/modeling/web/CapRateController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/farms/{farmId}/modeling/cap-rate/params")
fun getCapRateParams(@PathVariable farmId: Long): List<CapRateParamsUIModel> =
    service.getCapRateParams(farmId)
```

## Response

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
