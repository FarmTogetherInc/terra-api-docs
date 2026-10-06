# GET /api/ro/farms/{farmId}/water

Farm water overview: water districts (charts, WUE reports, ratings), subbasin, wells (on/nearby/section-grouped), water rights, dry wells, well stats.

Source: `facade/src/main/kotlin/com/farmtogether/facade/water/web/FarmWaterController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/water")
fun getWaterData(@PathVariable farmId: Long): FarmWaterResponseUIDto =
    farmWaterFacade.getWaterData(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/water/model/FarmWaterUIModels.kt
data class FarmWaterResponseUIDto(
    val name: String,
    val waterDistricts: List<WaterDistrictUIDto>,
    val subbasin: SubbasinUIDto?,
    val wellsOnProperty: List<WellUIDto>,
    val wellsOnNearbyProperties: List<WellUIDto>,
    val sectionWells: List<GroupedInaccurateWellsUIDto>,
    val waterRightsOnProperty: List<WaterRightUIDto>,
    val waterRightsOnNearbyProperties: List<WaterRightUIDto>,
    val dryWells: List<DryWellUIDto>,
    val wellStat: WellStatsUIDto,
)

data class GroupedInaccurateWellsUIDto(
    val section: SectionUIDto,
    val wells: List<WellUIDto>,
)

data class SectionUIDto(
    val name: String,
    val geometry: Geometry,
)

data class WaterDistrictUIDto(
    val name: String,
    val avgRatio10Years: BigDecimal?,
    val avgRatio15Years: BigDecimal?,
    val charts: List<ChartUIDto>,
    val wueReports: List<WaterDistrictWueReportUIDto>,
    val ratings: List<WaterDistrictRatingDto>
)

data class WaterDistrictWueReportUIDto(
    val id: Long,
    val year: Int,
    val acreage: BigDecimal,
    val annualDeliveries: BigDecimal,
    val url: String,
    val deliveriesPerAcre: BigDecimal?,
    val rawHtml: String,
)

data class ChartUIDto(
    val name: String,
    val values: List<ChartValueUIDto>,
)

data class ChartValueUIDto(
    val year: Int,
    val value: BigDecimal,
)

data class SubbasinUIDto(
    val name: String,
    val status: String?,
    val sustainableYield: Int?,
    val currentOverdraft: Int?,
    val irrigatedArea: Int?,
    val overdraftPercent: BigDecimal?,
)

data class WellUIDto(
    val number: String,
    val apn: String?,
    val matchStatus: LocationMatchStatus,
    val records: List<WellRecordUIDto>,
    val geometry: Point?,
    val accuracyDiameterMeters: BigDecimal?,
    val section: String?,
)

enum class LocationMatchStatus {
    ON_RECORD,
    BY_COORDS,
}

data class WellRecordUIDto(
    val date: String?,
    val recordType: String?,
    val wellYield: BigDecimal?,
    val wellDepth: BigDecimal?,
    val staticWaterLevel: BigDecimal?,
    val perforatedInterval: String?,
    val casingDiameter: BigDecimal?,
    val plannedFormerUse: String?,
)

data class WaterRightUIDto(
    val id: String?,
    val apn: String?,
    val matchStatus: LocationMatchStatus,
    val owner: String?,
    val ownerStatus: OwnerStatus?,
    val waterRightType: String?,
    val priorityDate: String?,
    val applicationFrom: String?,
    val status: String?,
    val sourceName: String?,
    val maxDirectDiversion: String?,
    val maxRequestedInYear: Double?,
    val geometry: Point?,
)

enum class OwnerStatus {
    MATCHED,
    NOT_MATCHED,
}

data class WellStatsUIDto(
    val counts: Counts,
    val averages: List<Average>,
    val depth: List<StatElement>,
    val yield: List<StatElement>,
) {
    data class Average(
        val staticWaterLevel: BigDecimal?,
        val casingDiameter: BigDecimal?,
        val year: Int?
    )

    data class StatElement(
        val minimum: BigDecimal?,
        val maximum: BigDecimal?,
        val average: BigDecimal?,
        val year: Int,
    )

    data class Counts(
        val total: Int,
        val staticWaterLevel: Int,
        val casingDiameter: Int,
        val depth: Int,
        val yield: Int,
    )
}

data class DryWellUIDto(
    val id: Long,
    val status: String,
    val shortageType: String?,
    val primaryUsages: String,
    val waterIssues: String?,
    val approximateIssueStartDate: LocalDate?,
    val wellDepth: String?,
    val wasIssueResolved: String?,
    val additionalInfo: String?,
    val wellToWaterDepth: String?,
    val reportDate: LocalDate,
    val statusType: String,
    val point: Point,
)

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WaterDistrict.kt
@Serializable
data class WaterDistrictRatingDto(
    val date: String,
    val ratings: ArrayList<WaterSecurityRating> = ArrayList(),
    val h2oRatings: ArrayList<EncompassH2OWaterSecurityRating> = ArrayList()
)

@Serializable
data class WaterSecurityRating(
    val name: String,
    val threatAssessment: Double,
    val reliability: Double,
    val costAndPricing: Double,
    val compositeAnalysis: Double,
    val finalWeightedRating: Double,
)

@Serializable
data class EncompassH2OWaterSecurityRating(
    val name: String,
    val rating: Double,
)
```

## Notes

- `Geometry`/`Point` are JTS types, serialized as GeoJSON.
