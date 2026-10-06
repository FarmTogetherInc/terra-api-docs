# Terra Read-Only API

Public read-only API surface of [Terra](https://terra.farmtogether.com) provided for external consumers under the read-only service agreement. Enforced by an explicit endpoint allowlist in the gateway (`terra` repo, `gateway/.../ReadOnlyEndpointFilter.kt`): anything not listed returns `403`.

## Base URL

```
https://terra.farmtogether.com/api/ro
```

Three surfaces:

| Prefix | Backend | Notes |
|---|---|---|
| `/api/ro/**` | facade | The 90 endpoints listed below (one doc file each) |
| `/api/ro/titiler/**` | titiler | Raster tiles — see [endpoints/titiler.md](endpoints/titiler.md) |
| `/api/ro/martin/**` | martin | Vector tiles — see [endpoints/martin.md](endpoints/martin.md) |

## Authentication

Send the static API token in the `Authorization` header, either as a raw UUID or as a Bearer token:

```
Authorization: 550e8400-e29b-41d4-a716-446655440000
Authorization: Bearer 550e8400-e29b-41d4-a716-446655440000
```

Tokens are issued by FarmTogether and have an inclusive expiry date (UTC). A token is valid through the end of its expiry date.

## Status codes

| Code | Meaning |
|---|---|
| 401 | Missing, unknown or expired token |
| 403 | Endpoint (method + path) is not in the read-only allowlist |
| 400 | titiler: `url` query param outside the allowed S3 prefix or contains `..` |
| 429 | More than 4 concurrent in-flight requests per token |

## Conventions

- JTS geometry fields (`Geometry`, `Point`, `LineString`) are serialized as GeoJSON objects (`{"type": ..., "coordinates": [...]}`).
- Fields named `*Wkt`, `wkt4326`, `geom`, `geometry4326`, `centroidWkt` are WKT strings in EPSG:4326.
- Dates are ISO-8601 (`2026-10-06`, `2026-10-06T12:00:00`).
- Paginated endpoints take a `PageRequest<Filters>` body (`{"filters": {...}, "page": {"size": 20, "number": 1}, "sort": {"column": "...", "direction": "ASC"}}`) and return `PageResponse<T>` (`{"data": [...], "totalPages": n, "totalElements": n, "page": {...}}`). Exact Kotlin definitions are included in each endpoint doc.
- Each endpoint doc contains the verbatim Kotlin source of the controller method and all request/response DTO classes — treat that code as the authoritative schema.

## Endpoints

### Farms

- [GET /farms/{farmId}](endpoints/farms.md) — farm overview
- [GET /farms/{farmId}/report](endpoints/farms-report.md) — short farm report
- [GET /farms/{farmId}/airtable-link](endpoints/farms-airtable-link.md)
- [GET /farms/{farmId}/price-history](endpoints/farms-price-history.md)
- [GET /farms/{farmId}/nearest-airport](endpoints/farms-nearest-airport.md)
- [GET /farms/{farmId}/nearest-cities](endpoints/farms-nearest-cities.md)
- [POST /farms/{farmId}/nearest-rejected](endpoints/farms-nearest-rejected.md)
- [GET /farms/{farmId}/owner/other-properties](endpoints/farms-owner-other-properties.md)
- [GET /farms/{farmId}/nearest-owned-properties](endpoints/farms-nearest-owned-properties.md)
- [GET /farms/{farmId}/climate](endpoints/farms-climate.md)
- [GET /farms/{farmId}/subsidence](endpoints/farms-subsidence.md)
- [GET /farms/{farmId}/water](endpoints/farms-water.md)
- [GET /farms/{farmId}/capital](endpoints/farms-capital.md)
- [GET /farms/{farmId}/buildings](endpoints/farms-buildings.md)
- [GET /farms/{farmId}/topography](endpoints/farms-topography.md)
- [GET /farms/{farmId}/soils](endpoints/farms-soils.md)
- [GET /farms/{id}/lidar](endpoints/farms-lidar.md)

### Hazards and satellite

- [GET /farms/{farmId}/hazards](endpoints/farms-hazards.md) — tornado/hail/wind event tracks
- [GET /farms/{farmId}/hazards/summary](endpoints/farms-hazards-summary.md)
- [GET /farms/{farmId}/hazards/burn-probabilities](endpoints/farms-hazards-burn-probabilities.md)
- [GET /farms/{farmId}/hazards/wildfire-probabilities](endpoints/farms-hazards-wildfire-probabilities.md)
- [GET /farms/{farmId}/hazards/flood-probabilities](endpoints/farms-hazards-flood-probabilities.md)
- [GET /farms/{farmId}/hazards/frost-probabilities](endpoints/farms-hazards-frost-probabilities.md)
- [GET /farms/{farmId}/sentinel/report](endpoints/farms-sentinel-report.md) — NDVI/NDMI
- [GET /farms/{farmId}/vegscape/report](endpoints/farms-vegscape-report.md) — weekly vegetation

### Crops and modeling

- [GET /farms/{farmId}/crops](endpoints/farms-crops.md)
- [GET /farms/{farmId}/crops/actual](endpoints/farms-crops-actual.md)
- [GET /farms/available-crop-years](endpoints/farms-available-crop-years.md)
- [GET /farms/available-crop-sources](endpoints/farms-available-crop-sources.md)
- [GET /crops/all](endpoints/crops-all.md)
- [POST /crops/usda/legend](endpoints/crops-usda-legend.md)
- [POST /crops/nlcd/legend](endpoints/crops-nlcd-legend.md)
- [GET /farms/{farmId}/modeling/cap-rate/params](endpoints/farms-modeling-cap-rate-params.md)
- [POST /farms/{farmId}/modeling/cap-rate/calculate](endpoints/farms-modeling-cap-rate-calculate.md)
- [GET /documents/modeling/{documentId}](endpoints/documents-modeling.md) — xlsx download

### Search

- [POST /farms/search](endpoints/farms-search.md)
- [POST /sales/search](endpoints/sales-search.md)
- [POST /farms/{farmId}/similar/sales/search](endpoints/farms-similar-sales-search.md)
- [POST /parcels/search](endpoints/parcels-search.md)
- [POST /listings/list](endpoints/listings-list.md)
- [POST /listings/price-changes](endpoints/listings-price-changes.md)
- [POST /locations/search](endpoints/locations-search.md)
- [POST /locations/washington/search](endpoints/locations-washington-search.md)

### Dictionaries

- [POST /dicts/airports](endpoints/dicts-airports.md)
- [POST /dicts/airports/nearest](endpoints/dicts-airports-nearest.md)
- [POST /dicts/avas](endpoints/dicts-avas.md)
- [POST /dicts/basins](endpoints/dicts-basins.md)
- [POST /dicts/buildings](endpoints/dicts-buildings.md)
- [POST /dicts/california-water-rights](endpoints/dicts-california-water-rights.md)
- [POST /dicts/cash-rent](endpoints/dicts-cash-rent.md)
- [POST /dicts/cities](endpoints/dicts-cities.md)
- [POST /dicts/cities/closest](endpoints/dicts-cities-closest.md)
- [POST /dicts/counties](endpoints/dicts-counties.md)
- [GET /dicts/counties/{id}/geometry](endpoints/dicts-counties-geometry.md)
- [POST /dicts/crops](endpoints/dicts-crops.md)
- [POST /dicts/crop-varieties](endpoints/dicts-crop-varieties.md)
- [POST /dicts/dry-wells](endpoints/dicts-dry-wells.md)
- [POST /dicts/ground-water-stations](endpoints/dicts-ground-water-stations.md)
- [POST /dicts/hazards/tornadoes](endpoints/dicts-hazards-tornadoes.md)
- [POST /dicts/hazards/hails](endpoints/dicts-hazards-hails.md)
- [POST /dicts/hazards/winds](endpoints/dicts-hazards-winds.md)
- [POST /dicts/land-survey/by-geom](endpoints/dicts-land-survey-by-geom.md)
- [POST /dicts/rejection-reasons](endpoints/dicts-rejection-reasons.md)
- [POST /dicts/source-types](endpoints/dicts-source-types.md)
- [POST /dicts/states](endpoints/dicts-states.md)
- [POST /dicts/tree-age-types](endpoints/dicts-tree-age-types.md)
- [POST /dicts/water-districts/search](endpoints/dicts-water-districts-search.md)
- [POST /dicts/water-districts/ratings](endpoints/dicts-water-districts-ratings.md)
- [POST /dicts/wells-data](endpoints/dicts-wells-data.md)
- [POST /dicts/wue-water-report/search](endpoints/dicts-wue-water-report-search.md)

### Soils and ArcGIS

- [POST /soils/stats](endpoints/soils-stats.md)
- [GET /soils/tasks/{id}](endpoints/soils-tasks.md)
- [POST /arcgis/soils/search](endpoints/arcgis-soils-search.md)
- [GET /arcgis/soils/tasks/{id}](endpoints/arcgis-soils-tasks.md)
- [POST /arcgis/almonds/search](endpoints/arcgis-almonds-search.md)
- [POST /arcgis/crops/search](endpoints/arcgis-crops-search.md)
- [GET /arcgis/crops/available-years](endpoints/arcgis-crops-available-years.md)
- [POST /arcgis/subbasins/get-attributes](endpoints/arcgis-subbasins-get-attributes.md)
- [POST /arcgis/opportunities-zones/intersects](endpoints/arcgis-opportunities-zones-intersects.md)
- [POST /arcgis/water-districts/get-attributes](endpoints/arcgis-water-districts-get-attributes.md)

### Climate

- [POST /climate/weather/historical/by-year](endpoints/climate-weather-historical-by-year.md)
- [POST /climate/weather/projected/by-year](endpoints/climate-weather-projected-by-year.md)

### LiDAR

- [GET /lidar/tree/tasks/list](endpoints/lidar-tree-tasks-list.md)
- [GET /lidar/tree/tasks/{id}](endpoints/lidar-tree-tasks.md)
- [GET /lidar/tree/tasks/{id}/data](endpoints/lidar-tree-tasks-data.md)

### Boundary and elevation

- [GET /boundary/{id}/get-data](endpoints/boundary-get-data.md)
- [POST /elevation/at-point](endpoints/elevation-at-point.md)

### Market data

- [GET /acrevalue/sales](endpoints/acrevalue-sales.md)
- [GET /acrevalue/sales/counties](endpoints/acrevalue-sales-counties.md)
- [GET /farmtogether/investment-goals/aggregation](endpoints/farmtogether-investment-goals-aggregation.md)

### Tiles

- [TiTiler raster tiles](endpoints/titiler.md)
- [Martin vector tiles](endpoints/martin.md)
