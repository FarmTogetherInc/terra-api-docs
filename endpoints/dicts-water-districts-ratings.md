# POST /api/ro/dicts/water-districts/ratings

Encompass water security ratings (plus H2O ratings) keyed by water district id.

Source: `facade/src/main/kotlin/com/farmtogether/dicts/app/web/WaterDistrictController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | WaterSecurityRatingsRequest | yes | Water district ids |

## Controller

```kotlin
@PostMapping(WATER_DISTRICTS_RATINGS_PATH)
override fun getWaterSecurityRatings(
    @RequestBody request: WaterSecurityRatingsRequest
): Map<Long, List<WaterDistrictRatingDto>> = encompassFileService.getRatings(request.waterDistrictIds)
```

## Request payload

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WaterDistrict.kt
@Serializable
class WaterSecurityRatingsRequest(
    val waterDistrictIds: List<Long>
)
```

## Response

`Map<Long, List<WaterDistrictRatingDto>>` keyed by water district id.

```kotlin
// facade-api/src/main/kotlin/com/farmtogether/dicts/api/models/WaterDistrict.kt
@Serializable
data class WaterDistrictRatingDto(
    val date: String,
    val ratings: ArrayList<WaterSecurityRating> = ArrayList(),
    val h2oRatings: ArrayList<EncompassH2OWaterSecurityRating> = ArrayList()
)

@Serializable
data class WaterSecurityRating(
    val name: String,
    val threatAssessment: Double,
    val reliability: Double,
    val costAndPricing: Double,
    val compositeAnalysis: Double,
    val finalWeightedRating: Double,
)

@Serializable
data class EncompassH2OWaterSecurityRating(
    val name: String,
    val rating: Double,
)
```
