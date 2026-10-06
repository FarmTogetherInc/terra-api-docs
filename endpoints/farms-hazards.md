# GET /api/ro/farms/{farmId}/hazards

Combined historical hazard event tracks (tornadoes, hail, wind) near the farm within a search radius over a year-history window.

Source: `facade/src/main/kotlin/com/farmtogether/facade/hazards/web/FarmHazardController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |
| search-radius-miles | query | Int | yes | Search radius around the farm, in miles |
| year-history | query | Int | yes | Number of years of hazard history to include |

## Controller

```kotlin
@GetMapping("/{farmId}/hazards")
fun getCombinedHazards(
    @PathVariable farmId: Long,
    @RequestParam("search-radius-miles") searchRadiusMiles: Int,
    @RequestParam("year-history") yearHistory: Int,
): CombinedHazardsResponse = service.getHazards(farmId, searchRadiusMiles, yearHistory)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/hazards/model/HazardUIModel.kt
data class CombinedHazardsResponse(
    val tornadoes: List<ShortHazardResponse>,
    val hails: List<ShortHazardResponse>,
    val winds: List<ShortHazardResponse>,
)

data class ShortHazardResponse(
    val id: Long,
    val geometry: LineString?,
    val date: LocalDate,
    val magnitude: BigDecimal,
    val lossText: String,
    val cropLossText: String,
    val lengthMiles: BigDecimal? = null,
    val measureType: String? = null,
)
```

## Notes

- `LineString` is a JTS type, serialized as GeoJSON.
