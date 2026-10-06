# GET /api/ro/acrevalue/sales/counties

County IDs for which Acrevalue sale data is available.

Source: `facade/src/main/kotlin/com/farmtogether/location/app/acrevalue/web/AcrevalueSalesController.kt`

## Parameters

None.

## Controller

```kotlin
@GetMapping(ACREVALUE_SALES_AVAILABLE_COUNTIES_PATH)
override fun getAvailableCountyIds(): List<Long> =
    acrevalueSalesService.getAvailableCountiesIds()
```

## Response

`List<Long>` — no DTO.
