# POST /api/ro/dicts/rejection-reasons

Deal rejection reasons filtered by ids. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/RejectionReasonController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | RejectionReasonRequest | yes | Ids filter (optional field) |

## Controller

```kotlin
@Cacheable("rejectionReasons")
@PostMapping(REJECTION_REASONS_PATH)
override fun getAll(@RequestBody request: RejectionReasonRequest): List<RejectionReasonDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/RejectionReason.kt
data class RejectionReasonRequest(
    val ids: List<Long>? = null
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/RejectionReason.kt
data class RejectionReasonDto(
    val id: Long,
    val name: String
)
```
