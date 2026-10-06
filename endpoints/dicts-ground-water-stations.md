# POST /api/ro/dicts/ground-water-stations

Groundwater monitoring stations near a geometry (within distance).

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/GroundWaterController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | GroundWaterRequest | yes | Filters |

## Controller

```kotlin
@PostMapping(GROUND_STATIONS_PATH)
override fun getAll(@RequestBody request: GroundWaterRequest): List<GroundWaterDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/GroundWater.kt
data class GroundWaterRequest(
    val distance: Double? = null,
    val wkt4326: String? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/GroundWater.kt
data class GroundWaterDto(
    val id: Long,
    val stnId: Int,
    val geom: String,
    val distanceInMeter: Int,
    val wellName: String?,
    val description: String?,
    val usage: String?,
    val type: String?,
    val depthInMeter: Int?,
    val comment: String?,
    val topPerforationInMeter: Int?,
    val botPerforationInMeter: Int?,
    val lastGseInMeter: Int?,
    val lastRpeInMeter: Int?,
    val monitoring: String?
)
```
