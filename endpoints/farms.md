# GET /api/ro/farms/{farmId}

Full farm overview: identity, acreage, county, owner, pricing, crops, geometry, parcels.

Source: `facade/src/main/kotlin/com/farmtogether/facade/overview/web/FarmOverviewController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/{farmId}")
fun farmInfo(@PathVariable farmId: Long): FarmOverviewUIModel =
    overviewService.getFarmOverview(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/overview/model/OverviewUIModels.kt
data class FarmOverviewUIModel(
    val id: Long,
    val name: String,
    val acres: BigDecimal?,
    val geometryAcres: BigDecimal?,
    val showAcresWarning: Boolean,
    val tillableAcres: BigDecimal?,
    val avas: List<String>?,
    val county: CountyUIDto?,
    val centroid: Point?,
    val owner: String?,
    val ownerAddress: String?,
    val mailingAddress: String?,
    val googleLink: String?,
    val totalPrice: BigDecimal?,
    val pricePerAcre: BigDecimal?,
    val qoz: List<String>?,
    val inRuralQoz: Boolean,
    val pricePerTillableAcre: BigDecimal?,
    val waterDistricts: List<String>,
    val crops: List<String>,
    val cropIds: List<Long>,
    val date: LocalDate?,
    val bucketText: String,
    val passReason: List<String>,
    val geometry: Geometry?,
    val parcels: List<ParcelUIDto>,
    val elevationFeet: Int?,
    val lidarTreeTaskId: String?,
    val urls: List<String>,
)

data class ParcelUIDto(
    val id: Long,
    val apn: String,
    val address: String?,
    val owner: String?,
)

// facade/src/main/kotlin/com/farmtogether/facade/common/model/CountyUIDto.kt
data class CountyUIDto(
    val name: String,
    val state: StateUIDto,
)

// facade/src/main/kotlin/com/farmtogether/facade/common/model/StateUIDto.kt
data class StateUIDto(
    val shortName: String,
)
```

## Notes

- `Geometry`/`Point` are JTS types, serialized as GeoJSON (`{"type", "coordinates"}`).
