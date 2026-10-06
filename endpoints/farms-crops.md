# GET /api/ro/farms/{farmId}/crops

Crop table for a farm (crops with geometries, acreage, percents) plus rendered image, filtered by optional source/year (defaults to USDA).

Source: `facade/src/main/kotlin/com/farmtogether/facade/crops/web/FarmCropsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |
| source | query | CropSource | no (default USDA) | Crop data source |
| year | query | Int | no | Crop year |

## Controller

```kotlin
@GetMapping("/{farmId}/crops")
fun getFarmCrops(
    @PathVariable farmId: Long,
    @RequestParam("source", required = false) source: CropSource?,
    @RequestParam("year", required = false) year: Int?
): FarmCropsUIModel =
    service.getFarmCrops(farmId, source ?: USDA, year)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/crops/model/FarmCropsUIModels.kt
data class FarmCropsUIModel(
    val year: Int?,
    val image: ImageUIModel?,
    val crops: CropUITable,
)

data class CropUITable(
    val items: List<CropUI>,
    val total: CropsTotal,
) {
    companion object {
        val EMPTY = CropUITable(
            items = emptyList(),
            total = CropsTotal(
                acres = BigDecimal.ZERO,
                percent = BigDecimal.ZERO,
            )
        )
    }
}

data class CropUI(
    val id: Int,
    val color: String,
    val name: String,
    val description: String? = null,
    val acres: BigDecimal?,
    val percent: BigDecimal?,
    val geometry: Geometry?,
)

data class CropsTotal(
    val acres: BigDecimal?,
    val percent: BigDecimal?,
)

// facade/src/main/kotlin/com/farmtogether/facade/common/model/ImageUIModel.kt
data class ImageUIModel(
    val type: String = "image",
    val base64bytes: String?,
    val coordinates: List<List<Double>>,
)

// facade/src/main/kotlin/com/farmtogether/facade/crops/model/CropSource.kt
enum class CropSource {
    USDA,
    CALIFORNIA_CROPS,
    NLCD,
}
```

## Notes

- `Geometry` is a JTS type, serialized as GeoJSON.
- Query param `source` takes `CropSource` enum values (USDA, CALIFORNIA_CROPS, NLCD).
