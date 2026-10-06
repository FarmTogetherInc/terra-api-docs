# GET /api/ro/crops/all

Full catalog of crops with their varieties.

Source: `facade/src/main/kotlin/com/farmtogether/facade/crops/web/CropListController.kt`

## Parameters

None.

## Controller

```kotlin
@GetMapping("/crops/all")
fun getAllCrops(): List<CropUIModel> =
    cropListService.getAll()
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/crops/model/CropUIModel.kt
data class CropUIModel(
    val id: Long,
    val name: String,
    val color: String,
    val varieties: List<VarietyUIModel>,
) {
    data class VarietyUIModel(
        val id: Long,
        val name: String,
    )
}
```
