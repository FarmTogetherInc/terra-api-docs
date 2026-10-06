# POST /sats/sky-watch

Search SkyWatch satellite scene metadata by ids or geometry (paged). Free DB read
(spatial intersects on `sats.sky_watch_scenes`); never calls the paid SkyWatch API.

## Request body

```kotlin
data class SkyWatchSearchRequest(
    val ids: List<Long>? = null,
    val wkt4326: String? = null,
    val page: Int = 1, // Page starts from 1
    val pageSize: Int = 10,
)
```

## Response

`List<SkyWatchMetadataDto>` (verbatim, `facade-api/.../satellites/api/models/SkyWatchDtos.kt`):

```kotlin
data class SkyWatchMetadataDto(
    val id: Long? = null,
    val skyWatchId: String = "",
    val wkt4326: String = "",
    val source: String = "",
    val productName: String = "",
    val resolution: BigDecimal = BigDecimal.ZERO,
    val startTime: ZonedDateTime = ZonedDateTime.now(),
    val endTime: ZonedDateTime = ZonedDateTime.now(),
    val previewUri: String? = null,
    val thumbnailUri: String? = null,
    val locationCoveragePercentage: BigDecimal = BigDecimal.ZERO,
    val areaSqKm: BigDecimal = BigDecimal.ZERO,
    val cost: BigDecimal = BigDecimal.ZERO,
    val resultCloudCoverPercentage: BigDecimal = BigDecimal.ZERO,
    val availableCredit: BigDecimal? = null,
)
```

`wkt4326` is a WKT geometry string (EPSG:4326). Use `skyWatchId` with
[GET /sats/thumbnail](sats-thumbnail.md) to fetch cached imagery
(`previewUri` points to the provider and is not accessible with your token).
