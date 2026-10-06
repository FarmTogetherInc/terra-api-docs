# POST /api/ro/dicts/basins

Drainage basins intersecting the given WKT4326 geometry. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/BasinController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | BasinRequest | yes | Geometry filter (optional field) |

## Controller

```kotlin
@Cacheable("basins")
@PostMapping(BASINS_PATH)
override fun getAll(@RequestBody request: BasinRequest): List<Basins> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/Basins.kt
data class BasinRequest(
    val wkt4326: String? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/Basins.kt
data class Basins(
    val id: Int,
    val wkbGeometry: String,
    val objectId: BigDecimal,
    val basinNumber: String,
    val basinSubb: String,
    val basinName: String,
    val basinSu1: String,
    val priority: String,
    val globalId: String,
)
```
