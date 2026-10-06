# POST /sats/planet

Search Planet satellite scene metadata by ids or geometry. Free DB read
(spatial intersects on `sats.planet_scenes`); never calls the paid Planet API.

## Request body

`PlanetSearchRequest` — at least one filter should be provided:

```kotlin
data class PlanetSearchRequest(
    val ids: List<Long>? = null,
    val wkt4326: String? = null,
)
```

## Response

`List<PlanetMetadataDto>` (verbatim, `facade-api/.../satellites/api/models/PlanetDtos.kt`):

```kotlin
data class PlanetMetadataDto(
    val id: Long? = null,
    val selfUrl: String,
    val assetsUrl: String,
    val thumbnailUrl: String,
    val wkt4326: String,
    val sceneId: String,
    val acquired: ZonedDateTime,
    val anomalousPixels: Long,
    val clearConfidencePercent: Int,
    val clearPercent: Int,
    val cloudCover: BigDecimal,
    val cloudPercent: Int,
    val columns: Long,
    val epsgCode: Int,
    val groundControl: Boolean,
    val gsd: BigDecimal,
    val heavyHazePercent: Int,
    val instrument: String?,
    val itemType: String,
    val lightHazePercent: Int,
    val originX: Int,
    val originY: Int,
    val pixelResolution: BigDecimal?,
    val provider: String,
    val published: ZonedDateTime,
    val publishingStage: String?,
    val qualityCategory: String,
    val rows: Long,
    val satelliteAzimuth: BigDecimal,
    val satelliteId: String,
    val shadowPercent: Int,
    val snowIcePercent: Int,
    val stripId: String,
    val sunAzimuth: BigDecimal,
    val sunElevation: BigDecimal,
    val updated: ZonedDateTime,
    val viewAngle: BigDecimal,
    val visibleConfidencePercent: Int,
    val visiblePercent: Int,
    val cameraId: String?,
    val groundControlRatio: Int,
)
```

`wkt4326` is a WKT geometry string (EPSG:4326). Use `sceneId` with
[GET /sats/thumbnail](sats-thumbnail.md) to fetch cached imagery.
