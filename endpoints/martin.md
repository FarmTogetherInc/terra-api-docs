# Martin vector tiles (`/api/ro/martin`)

Base: `https://terra.farmtogether.com/api/ro/martin` — proxies [Martin v0.13](https://github.com/maplibre/martin) (PostGIS vector tile server). The service itself only serves GET/HEAD.

## Authentication

Same static API token as the rest of the read-only API (`Authorization` header).

## Endpoints

| Pattern | Method(s) | Purpose |
|---|---|---|
| `/catalog` | GET | Catalog JSON: all available tile sources |
| `/{source_ids}/{z}/{x}/{y}` | GET | Mapbox Vector Tile (`.pbf`); comma-separated sources are merged (`/a,b/{z}/{x}/{y}`) |
| `/{source_ids}` | GET | TileJSON for one source or composite |
| `/health` | GET | Health check |

## Available layers

| Layer | Geometry | Properties |
|---|---|---|
| `building_footprints` | POLYGON | `capture_dates_range`, `ogc_fid`, `release` |
| `farm` | POINT (centroid) | `bucket_status`, `date_entered`, `id`, `name` |
| `usgs_region` | MULTIPOLYGON | `id`, `name` |

## Example

```
GET /api/ro/martin/building_footprints/12/655/1583
```
