# POST /api/ro/dicts/crop-varieties

Crop varieties filtered by ids/cropIds. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/CropVarietiesController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | CropVarietyRequest | yes | Filters |

## Controller

```kotlin
@Cacheable("cropVarieties")
@PostMapping(CROP_VARIETIES_PATH)
override fun getAll(@RequestBody request: CropVarietyRequest): List<CropVarietyDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/CropVariety.kt
data class CropVarietyRequest(
    val ids: List<Long>? = null,
    val cropIds: List<Long>? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/CropVariety.kt
@Serializable
data class CropVarietyDto(
    val id: Long,
    val varietyName: String,
    val cropId: Long,
    val cropName: String,
)
```
