# POST /api/ro/dicts/states

States filtered by ids/shortName/geometry intersection. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/StateController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | StateRequest | yes | Filters |

## Controller

```kotlin
@Cacheable("states")
@PostMapping(STATES_PATH)
override fun getAll(@RequestBody request: StateRequest): List<StateDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/States.kt
@Serializable
data class StateRequest(
    val ids: List<Long>? = null,
    val shortName: String? = null,
    val wkt4326: String? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/States.kt
@Serializable
data class StateDto(
    val id: Long,
    val name: String,
    val shortName: String,
)
```
