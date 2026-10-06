# POST /api/ro/dicts/crops

Crops filtered by ids/usdaIds/names. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/CropController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | CropRequest | yes | Filters |

## Controller

```kotlin
@Cacheable("crops")
@PostMapping(CROPS_PATH)
override fun getAll(@RequestBody request: CropRequest): List<CropDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/Crop.kt
@Serializable
data class CropRequest(
    val ids: List<Long>? = null,
    val usdaIds: List<Int>? = null,
    val names: List<String>? = null
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/Crop.kt
@Serializable
data class CropDto(
    val id: Long,
    val name: String,
    val usdaId: Int? = null,
    val color: String? = null,
    val cultivated: Boolean? = null,
    val chillingHours: CropChillingHours? = null,
    val type: CropType? = null,
)

@Serializable
data class CropChillingHours(
    val min: Int,
    val max: Int
)

enum class CropType {
    ROW,
    PERMANENT,
    OPEN_LAND,
    NON_CROP,
    UNKNOWN,
}
```
