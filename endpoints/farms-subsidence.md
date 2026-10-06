# GET /api/ro/farms/{farmId}/subsidence

Land subsidence statistics for the farm (large/small buffer averages and average inside geometry), null if unavailable.

Source: `facade/src/main/kotlin/com/farmtogether/facade/climate/web/FarmSubsidenceController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping
fun getSubsidence(@PathVariable farmId: Long): FarmSubsidenceResponseUIDto =
    service.getSubsidence(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/climate/model/FarmSubsidenceUIModels.kt
data class FarmSubsidenceResponseUIDto(
    val subsidenceStats: FarmSubsidenceStatsUIDto? = null,
)

data class FarmSubsidenceStatsUIDto(
    val largeBuffer: SubsidenceBuffer,
    val smallBuffer: SubsidenceBuffer,
    val averageInGeom: BigDecimal = BigDecimal.ZERO,
)

data class SubsidenceBuffer(
    val bufferSizeInMiles: Int,
    val averageValue: BigDecimal,
)
```
