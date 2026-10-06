# GET /api/ro/documents/modeling/{documentId}

Downloads a previously generated cap-rate xlsx report as a binary attachment.

Source: `facade/src/main/kotlin/com/farmtogether/facade/modeling/web/CapRateController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| documentId | path | String | yes | Identifier of the generated document (from `CapRateResultUIModel.documentId`) |

## Controller

```kotlin
@GetMapping("/documents/modeling/{documentId}")
fun downloadXlsx(@PathVariable documentId: String): ResponseEntity<ByteArray> {
    val fileWithName = xlsxDownloadService.download(documentId)
    return ResponseEntity.ok()
        .header("Content-Disposition", "attachment; filename=" + fileWithName.name)
        .body(fileWithName.content)
}
```

## Response

Binary xlsx file bytes (`Content-Disposition: attachment; filename=<name>`). No DTO.
