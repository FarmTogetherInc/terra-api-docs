# POST /api/ro/farms/{farmId}/similar/sales/search

Sales similar to the given farm (radius/price-band/water-district/crop criteria) plus price statistics.

Source: `facade/src/main/kotlin/com/farmtogether/facade/sales/web/SimilarSalesController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Reference farm id |
| body | body | SimilarSalesUIRequest | yes | Search criteria |

## Controller

```kotlin
@PostMapping("/farms/{farmId}/similar/sales/search")
fun getSimilarSales(
    @PathVariable farmId: Long,
    @RequestBody request: SimilarSalesUIRequest,
): SimilarSaleResponse =
    similarSalesService.getSimilarSales(farmId, request)
```

## Request payload

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/sales/model/SimilarSaleUIModel.kt
data class SimilarSalesUIRequest(
    val searchRadiusMiles: Int,
    val pricePerTillablePercentRange: Int,
    val sameWaterDistrict: Boolean,
    val sinceMonth: Int,
    val sources: List<SaleDto.SourceDto>,
    val cropIds: List<Long>,
    val tillablePercentGreaterThan: BigDecimal?,
)

// facade-api/src/main/kotlin/com/farmtogether/sales/api/models/SaleDto.kt (nested enum)
enum class SourceDto {
    LISTING,
    ACREVALUE,
    APPRAISAL,
}
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/sales/model/SimilarSaleUIModel.kt
data class SimilarSaleResponse(
    val sales: List<SimilarSaleUIModel>,
    val stats: SalesStatisticsModel?,
)

data class SimilarSaleUIModel(
    val id: Long,
    val acres: BigDecimal,
    val tillableAcres: BigDecimal,
    val price: BigDecimal,
    val date: LocalDate?,
    val centroid: Point?,
    val crops: List<CropUI>,
    val apns: List<String>,
    val county: CountyUIDto?,
    val buyers: List<String>,
    val sellers: List<String>,
    val distanceInMiles: BigDecimal?,
    val link: String,
    val buildingsCount: Int,
    val buildingsTotalAreaSquareFeet: Int,
    val waterDistricts: List<String>,
    val note: String?,
    val source: SaleDto.SourceDto,
) {
    val pricePerAcre = if (acres.isNotZero()) {
        price.divide(acres, MathContext.DECIMAL32)
    } else {
        null
    }

    val pricePerTillableAcre = if (tillableAcres.isNotZero()) {
        price.divide(tillableAcres, MathContext.DECIMAL32)
    } else {
        null
    }
}

data class SalesStatisticsModel(
    val count: Int,
    val price10th: Double,
    val priceMedian: Double,
    val price90th: Double,
)

// facade/src/main/kotlin/com/farmtogether/facade/crops/model/FarmCropsUIModels.kt
data class CropUI(
    val id: Int,
    val color: String,
    val name: String,
    val description: String? = null,
    val acres: BigDecimal?,
    val percent: BigDecimal?,
    val geometry: Geometry?,
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

- `Point`/`Geometry` are JTS types, serialized as GeoJSON.
