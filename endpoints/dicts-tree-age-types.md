# POST /api/ro/dicts/tree-age-types

Tree age type dictionary entries filtered by ids. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/TreeAgeTypeController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | TreeAgeTypeRequest | yes | Ids filter (optional field) |

## Controller

```kotlin
@Cacheable("treeAgeTypes")
@PostMapping(TREE_AGE_TYPES_PATH)
override fun getAll(@RequestBody request: TreeAgeTypeRequest): List<TreeAgeTypeDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/TreeAgeTypeDto.kt
data class TreeAgeTypeRequest(
    val ids: List<Long>? = null
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/TreeAgeTypeDto.kt
data class TreeAgeTypeDto(
    val id: Long,
    val name: String
)
```
