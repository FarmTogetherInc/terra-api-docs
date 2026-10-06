# GET /api/ro/farms/available-crop-years

Crop years available for a given data source (defaults to USDA).

Source: `facade/src/main/kotlin/com/farmtogether/facade/crops/web/FarmCropsController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| source | query | CropSource | no (default USDA) | Crop data source |

## Controller

```kotlin
@GetMapping("/available-crop-years")
fun getAvailableYears(@RequestParam("source", required = false) source: CropSource?): List<Int> =
    service.getAvailableYears(source ?: USDA)
```

## Response

`List<Int>` — no DTO.

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/crops/model/CropSource.kt
enum class CropSource {
    USDA,
    CALIFORNIA_CROPS,
    NLCD,
}
```
