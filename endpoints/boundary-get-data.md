# GET /api/ro/boundary/{id}/get-data

Aggregated boundary data (AVA, crops, soils, climate, elevation, etc.) for a boundary polygon by id.

Source: `facade/src/main/kotlin/com/farmtogether/provider/app/web/BoundaryController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| id | path | Long | yes | Boundary identifier |

## Controller

```kotlin
@GetMapping(GET_DATA_PATH)
override fun getData(@PathVariable id: Long): BoundaryDataResponse =
    boundaryFacade.getData(id)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/provider/api/BoundaryDataResponse.kt
data class BoundaryDataResponse(
    val avas: List<AvaInfo>? = null,
    val crops: Crops = Crops(),
    val opportunityZones: List<OpportunityZoneInfo>? = null,
    val soils: List<SoilInfo>? = null,
    val subBasin: SubBasinInfo? = null,
    val waterDistricts: List<WaterDistrictInfo>? = null,
    val buildings: List<BuildingInfo>? = null,
    val closestCity: CityInfo? = null,
    val county: CountyInfo? = null,
    val hazards: HazardsInfo = HazardsInfo(),
    val elevation: ElevationStats? = null,
    val subsidence: SubsidenceInfo? = null,
    val climate: ClimateInfo? = null,
) {

    data class Crops(
        val almonds: List<CropYear>? = null,
        val california: List<CropYear>? = null,
        val usda: List<CropYear>? = null,
        val nlcd: List<CropYear>? = null,
    )

    data class CropYear(
        val year: Int,
        val crops: List<CropPercent>,
    )

    data class CropPercent(
        val usdaId: Int?,
        val name: String,
        val areaSqMeters: Double,
        val yearPlanted: Int? = null,
    )

    data class AvaInfo(
        val id: Long,
        val name: String,
    )

    data class OpportunityZoneInfo(
        val id: Int,
        val tract: String,
        val isRural: Boolean = false,
    )

    data class SoilInfo(
        val areaSqMeters: Double,
        val iccdc: Int?,
        val niccdc: Int?,
        val mapUnitName: String,
        val slopeGradientDominantComponent: Int?,
        val mapUnitSymbol: String,
    )

    data class SubBasinInfo(
        val rvi: String,
        val name: String,
    )

    data class WaterDistrictInfo(
        val name: String,
    )

    data class BuildingInfo(
        val areaSqMeters: Double,
    )

    data class CityInfo(
        val name: String,
        val distanceMeters: Double,
        val state: String,
        val county: String,
    )

    data class CountyInfo(
        val name: String,
        val state: String,
    )

    data class HazardsInfo(
        val wildFire: String? = null,
    )

    data class SubsidenceInfo(
        val averageInGeom: Double,
        val averageAround: Double,
    )

    data class ClimateInfo(
        val historical: List<ByYearAggregatedTemperature>,
        val projected: List<ByYearAggregatedTemperature>,
    )

    data class ElevationStats(
        val min: Double,
        val max: Double,
        val average: Double,
    )

    data class ByYearAggregatedTemperature(
        val year: Int,
        val averageTemperature: Double?,
        val chillingHours: Int?,
    )
}
```
