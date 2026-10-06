# GET /api/ro/farms/available-crop-sources

All available crop data source enum values.

Source: `facade/src/main/kotlin/com/farmtogether/facade/crops/web/FarmCropsController.kt`

## Parameters

None.

## Controller

```kotlin
@GetMapping("/available-crop-sources")
fun getAvailableSources(): List<CropSource> =
    values().toList()
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/crops/model/CropSource.kt
enum class CropSource {
    USDA,
    CALIFORNIA_CROPS,
    NLCD,
}
```
