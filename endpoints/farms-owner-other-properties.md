# GET /api/ro/farms/{farmId}/owner/other-properties

Other parcels owned by the farm's owner: APN, acres, mailing address, land value, geometry.

Source: `facade/src/main/kotlin/com/farmtogether/facade/owner/web/FarmOwnerController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| farmId | path | Long | yes | Farm identifier |

## Controller

```kotlin
@GetMapping("/{farmId}/owner/other-properties")
fun parcelsList(@PathVariable farmId: Long): List<ParcelOwnerUIDto> =
    farmOwnerService.getParcels(farmId)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/owner/model/ParcelOwnerUIDto.kt
data class ParcelOwnerUIDto(
    val id: Long,
    val apn: String,
    val acres: BigDecimal?,
    val mailingAddress: String?,
    val landValue: BigDecimal?,
    val geometry: Geometry?,
)
```

## Notes

- `Geometry` is a JTS type, serialized as GeoJSON.
