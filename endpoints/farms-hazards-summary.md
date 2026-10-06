# GET /api/ro/farms/{farmId}/hazards/summary

Combined textual hazard summaries (wildfire, burn, flood, frost dates, subsidence) plus accumulated error messages for a farm.

Source: `facade/src/main/kotlin/com/farmtogether/facade/hazards/web/FarmHazardController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/{farmId}/hazards/summary")
fun getHazardSummaries(@PathVariable farmId: Long): HazardSummariesUIModel =
    service.getHazardSummaries(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/hazards/model/HazardUIModel.kt
data class HazardSummariesUIModel(
    val wildfire: HazardDescriptionUIModel? = null,
    val burn: HazardDescriptionUIModel? = null,
    val flood: HazardDescriptionUIModel? = null,
    val frostDates: FrostDatesUIModel? = null,
    val subsidence: SubsidenceUIModel? = null,
    val errors: List<String> = emptyList(),
)

data class HazardDescriptionUIModel(
    val description: String,
    val color: String,
)

data class SubsidenceUIModel(
    val feet: Double,
)

data class FrostDatesUIModel(
    val springFrostDate: String,
    val fallFrostDate: String,
    val growingSeasonDays: Int,
)
```
