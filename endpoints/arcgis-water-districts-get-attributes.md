# POST /api/ro/arcgis/water-districts/get-attributes

Water district attributes for all districts intersecting a requested geometry (WKT).

Source: `facade/src/main/kotlin/com/farmtogether/arcgis/app/waterdistricts/web/ArcGisWaterDistrictController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | FeatureRequestDto | yes | Query geometry |

## Controller

```kotlin
@PostMapping(WD_GET_ATTRIBUTES_PATH)
override fun getAttributes(@RequestBody request: FeatureRequestDto): List<WaterDistrictAttributesDto> =
    service.getAttributes(request)
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
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/WaterDistrictAttributesDto.kt
data class WaterDistrictAttributesDto(
    val name: String,
)
```
