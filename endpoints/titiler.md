# TiTiler raster tiles (`/api/ro/titiler`)

Base: `https://terra.farmtogether.com/api/ro/titiler` — proxies [TiTiler v0.17](https://developmentseed.org/titiler/) (COG raster tile service). Any method is allowed; POST exists only for zonal statistics/feature rendering.

## Authentication

Same static API token as the rest of the read-only API (`Authorization` header).

## `url` query parameter restriction

Every endpoint requires a `url` query parameter pointing to the dataset. **It must start with `https://terra-farmtogether.s3.amazonaws.com/tiffs/` and must not contain `..`**, otherwise the gateway returns `400`. Available datasets (public S3 bucket):

- `tiffs/usgs_dem_30m/combined.vrt` — USGS DEM elevation
- `tiffs/nlcd/{year}.tif` — NLCD land cover
- `tiffs/usda_crops/{year}.tif` — USDA CDL crops
- `tiffs/ca_crops/{year}.tif` — California crops
- `tiffs/hazards/whp.tif` — wildfire hazard potential
- `tiffs/hazards/burn.tif` — burn probability
- `tiffs/hazards/flood.tif` — flood probability
- `tiffs/hazards/subsidence-byte.tif`, `tiffs/subsidence-colored.tif` — subsidence

## Main endpoints

| Pattern | Method(s) | Purpose |
|---|---|---|
| `/cog/tiles/{z}/{x}/{y}.png?url=...&rescale=min,max&colormap_name=name` | GET | Map tiles (also `@2x`, other formats) |
| `/cog/tilejson.json?url=...` | GET | TileJSON descriptor |
| `/cog/info?url=...` | GET | Dataset metadata (bounds, stats, band info) |
| `/cog/bounds?url=...` | GET | Dataset bounds |
| `/cog/statistics?url=...` | GET, POST | Band statistics; POST body = GeoJSON Feature for zonal stats (`url` still in query) |
| `/cog/point/{lon},{lat}?url=...` | GET | Point value |
| `/cog/bbox/{minx},{miny},{maxx},{maxy}.png?url=...` | GET | Crop image by bbox |
| `/cog/feature` | POST | Image from GeoJSON Feature body (`url` still in query) |
| `/cog/preview.png?url=...` | GET | Full-dataset preview |
| `/cog/WMTSCapabilities.xml?url=...` | GET | OGC WMTS capabilities |
| `/tileMatrixSets` | GET | Supported tile matrix sets |
| `/healthz` | GET | Health check |

Full endpoint reference: https://developmentseed.org/titiler/endpoints/cog/

## Example

```
GET /api/ro/titiler/cog/tiles/8/91/197.png?url=https://terra.farmtogether.s3.amazonaws.com/tiffs/nlcd/2019.tif&nodata=0
```
