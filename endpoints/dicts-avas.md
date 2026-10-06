# POST /api/ro/dicts/avas

American Viticultural Areas intersecting the given WKT4326 geometry (all AVAs if no geometry). Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/AvaController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | AvaRequest | yes | Geometry filter (optional field) |

## Controller

```kotlin
@Cacheable("avas")
@PostMapping(AVAS_PATH)
override fun getAll(@RequestBody request: AvaRequest): List<AvaDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/Ava.kt
data class AvaRequest(
    val wkt4326: String? = null
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/Ava.kt
data class AvaDto(
    val id: Int,
    val wkt: String,
    val avaId: String,
    val name: String,
    val aka: String?,
    val created: LocalDate,
    val removed: String?,
    val county: String?,
    val state: String,
    val within: String?,
    val contains: String?,
    val petitioner: String?,
    val cfrAuthor: String?,
    val cfrIndex: String,
    val cfrRevisionHistory: String,
    val approvedMaps: String,
    val boundaryDescription: String,
    val usedMaps: String?,
    val validStart: String?,
    val validEnd: String?,
    val lcsh: String?,
    val sameas: String?,
)
```
