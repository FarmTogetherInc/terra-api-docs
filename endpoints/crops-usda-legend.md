# POST /api/ro/crops/usda/legend

USDA crop legend (crop name/color/acres) for a map bounding box and year.

Source: `facade/src/main/kotlin/com/farmtogether/facade/crops/web/CropLegendController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | UsdaLegendRequest | yes | Bounds and year |

## Controller

```kotlin
@PostMapping("/crops/usda/legend")
fun getCropLegend(@RequestBody request: UsdaLegendRequest): List<UsdaLegendUIModel> =
    usdaLegendService.getLegend(request)
```

## Request payload

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/crops/model/UsdaLegend.kt
data class UsdaLegendRequest(
    val bounds: UsdaLegendBoundsRequest,
    val year: Int,
)

data class UsdaLegendBoundsRequest(
    val sw: Coordinates,
    val ne: Coordinates,
) {
    data class Coordinates(
        val lng: Double,
        val lat: Double,
    )
}
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/crops/model/UsdaLegend.kt
data class UsdaLegendUIModel(
    val name: String,
    val color: String,
    val acres: BigDecimal,
)
```
