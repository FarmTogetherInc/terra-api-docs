# POST /api/ro/dicts/wells-data

California well completion records near a geometry / by APNs / by county. Server-side cached.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/WellDataController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | WellDataRequest | yes | Filters |

## Controller

```kotlin
@Cacheable("wellsData")
@PostMapping(WELLS_DATA_PATH)
override fun getAll(@RequestBody request: WellDataRequest): List<WellDataDto> =
    service.getAll(request)
        .toDtos()
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WellData.kt
data class WellDataRequest(
    val wkt4326: String? = null,
    val distanceInMeters: Int? = null,
    val apns: List<String>? = null,
    val countyName: String? = null,
)
```

## Response

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WellData.kt
data class WellDataDto(
    val wcrNumber: String,
    val legacyLogNumber: String?,
    val regionOffice: String?,
    val countyName: String?,
    val localPermitAgency: String?,
    val permitDate: LocalDate?,
    val permitNumber: String?,
    val ownerAssignedWellNumber: String?,
    val wellLocation: String?,
    val city: String?,
    val plannedUseFormerUse: String?,
    val drillerName: String?,
    val drillerLicenseNumber: String?,
    val recordType: String?,
    val decimalLatitude: Double?,
    val decimalLongitude: Double?,
    val methodOfDeterminationLl: String?,
    val llAccuracy: String?,
    val horizontalDatum: String?,
    val groundSurfaceElevation: Double?,
    val elevationAccuracy: String?,
    val elevationDeterminationMethod: String?,
    val verticalDatum: String?,
    val township: String?,
    val range: String?,
    val section: String?,
    val baselineMeridian: String?,
    val apn: String?,
    val dateWorkEnded: LocalDateTime?,
    val workflowStatus: String?,
    val receivedDate: LocalDate?,
    val totalDrillDepth: Double?,
    val totalCompletedDepth: Double?,
    val topOfPerforatedInterval: Double?,
    val bottomOfPerforatedInterval: Double?,
    val casingDiameter: Double?,
    val drillingMethod: String?,
    val fluid: String?,
    val staticWaterLevel: Double?,
    val totalDrawdown: Double?,
    val testType: String?,
    val pumpTestLength: Double?,
    val wellYield: Double?,
    val wellYieldUnitOfMeasure: String?,
    val otherObservations: String?,
    val geometry: String?,
    val recordTypeSimplified: String?,
    val baselineMeridianCode: String?,
)
```
