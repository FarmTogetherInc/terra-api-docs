# POST /api/ro/dicts/california-water-rights

California water rights near a geometry (within distance) or by APNs. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/CaliforniaWaterRightController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | CaliforniaWaterRightRequest | yes | Filters |

## Controller

```kotlin
@Cacheable("californiaWaterRights")
@PostMapping(WATER_RIGHT_PATH)
override fun getAll(@RequestBody request: CaliforniaWaterRightRequest): List<CaliforniaWaterRightDto> =
    service.getAll(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/CaliforniaWaterRight.kt
data class CaliforniaWaterRightRequest(
    val distanceInMeters: Int? = null,
    val wkt4326: String? = null,
    val apns: List<String>? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/CaliforniaWaterRight.kt
data class CaliforniaWaterRightDto(
    val id: String?,
    val apn: String?,
    val owner: String?,
    val waterRightType: String?,
    val priorityDate: String?,
    val applicationFrom: String?,
    val status: String?,
    val sourceName: String?,
    val maxDirectDiversion: String?,
    val maxRequestedInYear: Double?,
    val geometry: String?,
)
```
