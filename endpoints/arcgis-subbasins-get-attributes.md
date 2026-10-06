# POST /api/ro/arcgis/subbasins/get-attributes

Attributes of the single subbasin intersecting a requested geometry (WKT), or null if none.

Source: `facade/src/main/kotlin/com/farmtogether/arcgis/app/subbasins/web/SubbasinController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | FeatureRequestDto | yes | Query geometry |

## Controller

```kotlin
@PostMapping(SUBBASIN_GET_ATTRIBUTES_PATH)
override fun getAttributes(@RequestBody request: FeatureRequestDto): SubbasinAttributesDto? =
    service.searchOne(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/FeatureRequestDto.kt
data class FeatureRequestDto(
    val geometry: String, // WKT with SRID 4326
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/SubbasinAttributesDto.kt
data class SubbasinAttributesDto(
    val rvi: String,
    val name: String,
    val status: String?,
    val sustainableYield: Int?,
    val currentOverdraft: Int?,
    val irrigatedArea: Int?,
    val overdraftPercent: BigDecimal?,
)
```
