# GET /farms/{farmId}/sats

SkyWatch scenes intersecting the farm geometry, with per-scene pricing.
Free DB read; never calls the paid provider.

## Path parameters

| Name | Type | Notes |
|---|---|---|
| `farmId` | long | |

## Query parameters

| Name | Type | Default | Notes |
|---|---|---|---|
| `page` | int | 1 | |
| `size` | int | 10 | |

## Response

`FarmSatsUIDto` (verbatim, `facade/src/main/kotlin/com/farmtogether/facade/sats/model/`):

```kotlin
data class FarmSatsUIDto(
    val skyWatchScenes: List<SkyWatchUIDto>,
)

data class SkyWatchUIDto(
    val sceneId: String,
    val provider: SatelliteProvider = SKYWATCH,
    val price: BigDecimal,
    val source: String,
    val resolution: BigDecimal,
    val cloudCoverage: BigDecimal,
    val sceneDateTime: ZonedDateTime,
    val previewUri: String?,
    val geometry: Geometry?,
)
```

`geometry` is the scene envelope as GeoJSON (JTS `Geometry` on the wire).
Use `sceneId` with [GET /sats/thumbnail](sats-thumbnail.md) to fetch cached imagery.
