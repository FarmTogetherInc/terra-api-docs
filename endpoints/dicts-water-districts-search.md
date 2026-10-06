# POST /api/ro/dicts/water-districts/search

Water districts filtered by ids/names. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/WaterDistrictController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | WaterDistrictRequest | yes | Filters |

## Controller

```kotlin
@Cacheable(WD_CACHE)
@PostMapping(WATER_DISTRICTS_PATH)
override fun getAll(@RequestBody request: WaterDistrictRequest): List<WaterDistrictDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WaterDistrict.kt
data class WaterDistrictRequest(
    val ids: List<Long>? = null,
    val names: List<String>? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WaterDistrict.kt
@Serializable
data class WaterDistrictDto(
    val id: Long,
    val name: String,
    val deliveries: Map<Int, Int>,
    val avgRatio10Years: BigDecimal?,
    val avgRatio15Years: BigDecimal?,
)
```

## Notes

- `BigDecimal` fields in this file are serialized as plain strings (`BigDecimalSerializer`).
