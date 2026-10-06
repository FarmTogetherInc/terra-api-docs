# GET /api/ro/farms/{farmId}/hazards/frost-probabilities

Frost probability hazard map for a farm: rendered image, legend, description, centroid.

Source: `facade/src/main/kotlin/com/farmtogether/facade/hazards/web/FarmHazardController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/{farmId}/hazards/frost-probabilities")
fun getFrostProbability(@PathVariable farmId: Long): HazardUIModel? =
    service.getFrostProbability(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/hazards/model/HazardUIModel.kt
data class HazardUIModel(
    val farmId: Long,
    val name: String,
    val source: String?,
    val legend: List<LegendItemUIDto>,
    val description: String? = null,
    val centroid: Point?,
    val image: ImageUIModel?,
)

data class LegendItemUIDto(
    val color: String,
    val description: String,
)

// facade/src/main/kotlin/com/farmtogether/facade/common/model/ImageUIModel.kt
data class ImageUIModel(
    val type: String = "image",
    val base64bytes: String?,
    val coordinates: List<List<Double>>,
)
```

## Notes

- `Point` is a JTS type, serialized as GeoJSON.
- `image.base64bytes` is a base64-encoded PNG; `coordinates` are the image corner coordinates for map overlay.
