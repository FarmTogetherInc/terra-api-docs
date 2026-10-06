# GET /api/ro/farms/{farmId}/capital

Capital allocation goal (allocation/total invested/dry powder) and allocation timeline by hold period and target IRR. Data is portfolio-wide, `farmId` is not used for lookup.

Source: `facade/src/main/kotlin/com/farmtogether/facade/capital/web/FarmCapitalController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier (not used for lookup) |

## Controller

```kotlin
@GetMapping("/capital")
fun getCapitalData(@PathVariable farmId: Long): FarmCapitalUIModels.Response =
    farmCapitalService.getCapitalData(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/capital/model/FarmCapitalUIModels.kt
object FarmCapitalUIModels {
    data class Response(
        val allocationGoal: Goal,
        val allocationTimeline: Timeline,
    )

    data class Goal(
        val allocation: Long,
        val totalInvested: Long,
        val dryPowder: Long,
    )

    data class Timeline(
        val holdPeriodSources: List<Source>,
        val targetIrrSources: List<Source>,
        val valuesByHold: List<Map<String, Any>>,
        val valuesByIrr: List<Map<String, Any>>,
    )

    data class Source(
        val name: String,
        val value: String,
    )
}
```
