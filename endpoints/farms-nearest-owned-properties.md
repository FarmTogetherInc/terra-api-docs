# GET /api/ro/farms/{farmId}/nearest-owned-properties

FarmTogether-owned properties nearest to the farm within a search radius.

Source: `facade/src/main/kotlin/com/farmtogether/facade/common/web/NearestFTPropertiesController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |
| search-radius-miles | query | Int | yes | Search radius in miles |
| page-size | query | Int | no (default 3) | Max number of nearest owned properties to return |

## Controller

```kotlin
@GetMapping("/{farmId}/nearest-owned-properties")
fun getFarmNearestOwnedProperties(
    @PathVariable farmId: Long,
    @RequestParam("search-radius-miles") searchRadiusMiles: Int,
    @RequestParam("page-size", defaultValue = "3") pageSize: Int,
): FarmNearestOwnedPropertyResponse =
    ownedFarmService.getNearestFTProperties(farmId, searchRadiusMiles, pageSize)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/common/model/FarmNearestObjectUIModels.kt
data class FarmNearestOwnedPropertyResponse(
    val totalSize: Int,
    val data: List<FarmNearestOwnedPropertyUIModel>,
)

data class FarmNearestOwnedPropertyUIModel(
    val farmId: Long,
    val name: String,
    val wonDate: LocalDate?,
    val distanceInMiles: BigDecimal,
    val cropTypes: List<String>,
    val geometry: Geometry?,
    val centroid: Point?,
    val isBespokeDeal: Boolean,
)
```

## Notes

- `Geometry`/`Point` are JTS types, serialized as GeoJSON.
