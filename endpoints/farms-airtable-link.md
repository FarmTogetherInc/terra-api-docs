# GET /api/ro/farms/{farmId}/airtable-link

Airtable record link for the farm, or null.

Source: `facade/src/main/kotlin/com/farmtogether/facade/overview/web/FarmOverviewController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/{farmId}/airtable-link")
fun airtableLink(@PathVariable farmId: Long): String? =
    airtableLinkDao.getLink(farmId)
```

## Response

Plain `String?` (URL) — no DTO.
