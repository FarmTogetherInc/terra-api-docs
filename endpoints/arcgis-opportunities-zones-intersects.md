# POST /api/ro/arcgis/opportunities-zones/intersects

Opportunity zones intersecting a requested geometry (WKT), with zone metadata.

Source: `facade/src/main/kotlin/com/farmtogether/arcgis/app/opzones/web/OpportunitiesZoneController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | OpportunitiesZoneRequest | yes | Query geometry |

## Controller

```kotlin
@PostMapping(OPPORTUNITIES_ZONES_GET_BY_GEOM_PATH)
override fun search(@RequestBody request: OpportunitiesZoneRequest): List<OpportunitiesZoneDto> =
    service.search(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/OpportunitiesZoneDtos.kt
data class OpportunitiesZoneRequest(
    val wkt4326: String,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/arcgis/api/models/OpportunitiesZoneDtos.kt
data class OpportunitiesZoneDto(
    val ogcFid: Int,
    val wktGeometry: String?,
    val objectid: BigDecimal?,
    val geoid10: String?,
    val state: String?,
    val county: String?,
    val tract: String?,
    val stusab: String?,
    val stateName: String?,
    val isRural: Boolean = false,
)
```
