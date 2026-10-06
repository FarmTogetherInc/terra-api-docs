# GET /api/ro/arcgis/crops/available-years

Years for which crop layer data is available.

Source: `facade/src/main/kotlin/com/farmtogether/arcgis/app/crops/CropsController.kt`

## Parameters

None.

## Controller

```kotlin
@GetMapping(CROPS_AVAILABLE_YEARS_PATH)
override fun availableYears(): List<Int> =
    service.availableYears()
```

## Response

`List<Int>` — no DTO.
