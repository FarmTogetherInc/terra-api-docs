# POST /api/ro/dicts/source-types

Water source types filtered by ids/name. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/SourceTypeController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | SourceTypeRequest | yes | Filters |

## Controller

```kotlin
@Cacheable("sourcesTypes")
@PostMapping(SOURCE_TYPES_PATH)
override fun getAll(@RequestBody request: SourceTypeRequest): List<SourceTypeDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/SourceType.kt
@Serializable
data class SourceTypeRequest(
    val ids: List<Long>? = null,
    val name: String? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/SourceType.kt
@Serializable
data class SourceTypeDto(
    val id: Long,
    val name: String,
)
```
