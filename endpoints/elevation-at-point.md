# POST /api/ro/elevation/at-point

Elevation in feet at a given point (GeoJSON Point body); null if elevation cannot be resolved.

Source: `facade/src/main/kotlin/com/farmtogether/facade/elevation/ElevationController.kt`

## Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| body | body | ElevationUIRequest | yes | Point to query |

## Controller

```kotlin
@PostMapping("/elevation/at-point")
fun getElevationAtPoint(@RequestBody request: ElevationUIRequest): ElevationUIResponse? =
    service.getElevationAtPoint(request)
```

## Request payload

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/elevation/ElevationUIModels.kt
data class ElevationUIRequest(
    val point: Point,
)
```

## Response

```kotlin
// facade/src/main/kotlin/com/farmtogether/facade/elevation/ElevationUIModels.kt
data class ElevationUIResponse(
    val elevationFeet: Double,
)
```

## Notes

- `Point` is a JTS type, serialized as GeoJSON (`{"type": "Point", "coordinates": [lon, lat]}`).
