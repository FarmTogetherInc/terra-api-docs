# GET /api/ro/farmtogether/investment-goals/aggregation

Investment goal aggregations (allocation goal and timeline projections) for a US state.

Source: `facade/src/main/kotlin/com/farmtogether/connector/app/web/AggregationsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| stateShortCode | query | String | yes | US state short code (e.g. "IA") |

## Controller

```kotlin
@GetMapping(INVESTMENT_GOALS_AGGREGATIONS_PATH)
override fun getByState(@RequestParam stateShortCode: String): InvestmentGoalAggregations.Result =
    aggregationsService.calculateAggregations(stateShortCode)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/connector/api/models/InvestmentGoalAggregations.kt
object InvestmentGoalAggregations {

    data class Result(
        val allocationGoal: AllocationGoal,
        val allocationTimeline: List<Timeline>,
    )

    data class AllocationGoal(
        val allocation: Long,
        val totalInvested: Long,
        val dryPowder: Long,
    )

    data class Timeline(
        val months: Int,
        val valuesByHold: List<TimelineByHoldValues>,
        val valuesByIrr: List<TimelineByIrrValues>,
    )

    data class TimelineByHoldValues(
        val holdPeriodYears: Int?,
        val allocationAmount: Long,
    )

    data class TimelineByIrrValues(
        val targetIrr: Int?,
        val allocationAmount: Long,
    )
}
```
