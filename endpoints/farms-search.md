# POST /api/ro/farms/search

Paginated search over farms with flexible filters (geometry, APNs, owner, bucket status, distance, QOZ flags, etc.), returns full farm read models.

Source: `facade/src/main/kotlin/com/farmtogether/farms/app/web/FarmReadController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | PageRequest&lt;FarmFiltersDto&gt; | yes | Filters + pagination |

## Controller

```kotlin
@PostMapping(FARM_SEARCH_PATH)
override fun search(@RequestBody request: PageRequest<FarmFiltersDto>): PageResponse<FarmReadDto> =
    facade.search(request)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/farms/api/models/FarmFiltersDto.kt
@Serializable
data class FarmFiltersDto(
    val withMissingGeometry: Boolean? = null,
    val geom: String? = null,
    val apns: List<String>? = null,
    val owner: String? = null,
    val ids: List<Long>? = null,
    val bucket: BucketFilter? = null,
    val distance: DistanceFilter? = null,
    val delisted: Boolean? = null,
    val rejectedDateGreaterThan: LocalDate? = null,
    val passReason: PassReasonFilter? = null,
    val withCentroid: Boolean? = null,
    val inQoz: Boolean? = null,
    val inRuralQoz: Boolean? = null,
)

@Serializable
data class BucketFilter(
    val bucketStatus: BucketStatus,
    val excludeCapitalSource: String? = null,
)

@Serializable
data class DistanceFilter(
    val wkt4326: String,
    val lessThanMeters: Int,
)

@Serializable
data class PassReasonFilter(
    val notContains: String? = null,
)

// facade-api/src/main/kotlin/com/farmtogether/farms/api/models/BucketStatus.kt
enum class BucketStatus {
    NEW_DEAL,

    INFORMATION_INCOMPLETE,
    LONG_LIST_NEEDS_REVIEW,
    BACK_BURNER,

    PENDING_MODEL,
    PENDING_PIR,
    SHORT_LIST_NEEDS_REVIEW,
    DESK_DILIGENCE,
    BOOTS_ON_THE_GROUND,
    SHORT_LIST_INFORMATION_INCOMPLETE,

    PENDING_FINAL_MODEL,
    PENDING_FINAL_PIR,
    INVESTMENT_COMMITTEE_NEEDS_REVIEW,

    PREPARING_OFFER,
    WAITING_FOR_SELLER_RESPONSE,
    WON,

    REJECTED,
    LOST_DEAL,
    DELETED,

    FUND_OR_BESPOKE_REVIEW,

    NO_LONGER_ON_MARKET,
    APPROVED_LIVE,
    APPROVED_PENDING_CLOSED,

    REFER_TO_PARTNER,
    BROKER_PENDING,
}

// utils/pagination-api/src/main/kotlin/com/farmtogether/common/page/PageModels.kt
@Serializable
data class PageRequest<R>(
    val filters: R? = null,
    val page: Pagination? = null,
    val sort: SortOrder? = null,
)

@Serializable
data class Pagination(
    val size: Int,
    val number: Int = 1,
)

@Serializable
data class SortOrder(
    val column: String,
    val direction: SortDirection = SortDirection.ASC,
)

enum class SortDirection {
    ASC,
    DESC,
}
```

## Response

```kotlin
// utils/pagination-api/src/main/kotlin/com/farmtogether/common/page/PageModels.kt
@Serializable
data class PageResponse<T>(
    val data: List<T>,
    val totalPages: Int,
    val totalElements: Long,
    val page: Pagination?,
) {
    fun <R> mapAll(f: (List<T>) -> List<R>): PageResponse<R> = PageResponse(
        data = f(data),
        totalPages,
        totalElements,
        page
    )

    fun <R> map(f: (T) -> (R)): PageResponse<R> = mapAll { data -> data.map(f) }

    companion object {
        fun <T> empty() = PageResponse<T>(
            data = emptyList(),
            totalPages = 0,
            totalElements = 0,
            page = null,
        )
    }
}

// facade-api/src/main/kotlin/com/farmtogether/farms/api/models/FarmDtos.kt
@Serializable
data class FarmReadDto(
    val id: Long,
    val created: LocalDateTime,
    val updated: LocalDateTime?,
    val dateEntered: LocalDate?,
    val farmName: String,
    val urls: List<String>,
    val status: String,
    val acreage: BigDecimal,
    val tillableAcreage: BigDecimal?,
    val price: BigDecimal,
    val fileUrl: String?,
    val plantings: String?,
    val comment: String?,
    val crops: List<FarmCropReadDto>,
    val cropsSummary: CropsSummaryDto,
    val apns: List<String>,
    val parcels: List<FarmParcelDto>,
    val county: CountyDto?,
    val waterText: String?,
    val waterDistricts: List<WaterDistrictDto>,
    val geometry4326: String?,
    val centroid: String?,
    val address: String?,
    val owner: String?,
    val ownerAddress: String?,
    val notes: String?,
    val delisted: LocalDateTime?,
    val sourceTypeId: Long?,
    val dealName: String?,
    val bucketStatus: BucketStatus,
    val airtableBucketStatus: String,
    val bucketStatusDateTime: LocalDateTime?,
    val rejectedReasons: String?,
    val rejectedDate: LocalDate?,
    val adjustedStatusHistory: Map<BucketStatus, LocalDate>,
    val capitalSource: String?,
    val locationStatus: LocationStatusDto?,
    val irrigated: Boolean,
    val rowCropsGrossReturn: BigDecimal?,
    val description: String?,
    val passReasonModifiedBy: String?,
    val isBespokeDeal: Boolean,
    val elevation: Int?,
    val adviserLabels: List<String>,
    val lidarTreeTaskId: String?,
    val vegscapeReportId: Long?,
    val sentinelReportId: Long?,
    val listings: List<FarmListingDto>,
    val inQoz: Boolean? = null,
    val inRuralQoz: Boolean? = null,
    val qozTracts: List<String> = emptyList(),
)

@Serializable
data class LocationStatusDto(
    val message: String,
    val description: String?,
)

@Serializable
data class FarmCropReadDto(
    val crop: CropDto,
    val variety: CropVarietyDto?,
    val acreage: BigDecimal?,
    val yearPlanted: Int?,
)

@Serializable
data class FarmListingDto(
    val listingId: Long,
    val siteName: String?,
    val created: LocalDateTime,
)

// facade-api/src/main/kotlin/com/farmtogether/farms/api/models/CropsSummaryDto.kt
@Serializable
data class CropsSummaryDto(
    val tillableAcres: BigDecimal?,
    val usda: Details?,
    val california: Details?,
    val reGrid: Details?,
    val actual: Details?,
    val almonds: Details?,
) {
    @Serializable
    data class Details(
        val main: CropDetails?,
        val all: List<CropDetails>,
        val tillableAcres: BigDecimal?,
    )

    @Serializable
    data class CropDetails(
        val cropId: Long?,
        val cropName: String,
        val percent: BigDecimal?,
        val yearPlanted: Int?,
    )
}

// facade-api/src/main/kotlin/com/farmtogether/farms/api/models/FarmParcelDto.kt
@Serializable
data class FarmParcelDto(
    val id: Long,
    val apn: String,
    val parcel: ParcelDto?,
)

// facade-api/src/main/kotlin/com/farmtogether/farms/api/parcels/models/ParcelDto.kt
@Serializable
data class ParcelDto(
    val id: Long,
    val apn: String,
    val countyId: Long,
    val geometryWkt: String,
    val acres: BigDecimal,
    val address: String?,
    val owner: String?,
)

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/County.kt
@Serializable
data class CountyDto(
    val id: Long,
    val name: String,
    val state: StateDto,
    val fips: String,
)

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/States.kt
@Serializable
data class StateDto(
    val id: Long,
    val name: String,
    val shortName: String,
)

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WaterDistrict.kt
@Serializable
data class WaterDistrictDto(
    val id: Long,
    val name: String,
    val deliveries: Map<Int, Int>,
    val avgRatio10Years: BigDecimal?,
    val avgRatio15Years: BigDecimal?,
)

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/Crop.kt
@Serializable
data class CropDto(
    val id: Long,
    val name: String,
    val usdaId: Int? = null,
    val color: String? = null,
    val cultivated: Boolean? = null,
    val chillingHours: CropChillingHours? = null,
    val type: CropType? = null,
)

@Serializable
data class CropChillingHours(
    val min: Int,
    val max: Int
)

enum class CropType {
    ROW,
    PERMANENT,
    OPEN_LAND,
    NON_CROP,
    UNKNOWN,
}

// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/CropVariety.kt
@Serializable
data class CropVarietyDto(
    val id: Long,
    val varietyName: String,
    val cropId: Long,
    val cropName: String,
)
```

## Notes

- `geometry4326`/`centroid`/`geometryWkt` are WKT strings.
- Known sort column: `calculatedRejectedDate`.
