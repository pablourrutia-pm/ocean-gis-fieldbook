# Data source note

**There is no data file in this folder, and that is the point of Module 4.**

Module 3 downloaded an 11 MB zip and stored it here because Colab wipes uploads. Module 4 stores nothing, because the raster is read over HTTP a few kilobytes at a time and never lands on disk.

---

## The raster

**Asset:** `https://sentinel-cogs.s3.us-west-2.amazonaws.com/sentinel-s2-l2a-cogs/30/S/VF/2025/8/S2C_30SVF_20250815_0_L2A/TCI.tif`
**Scene id:** `S2C_30SVF_20250815_0_L2A`
**Found via:** Earth Search STAC API (Element 84), collection `sentinel-2-l2a`, 2026-09-10
**Sensed:** 2025-08-15
**Cloud cover:** 0.0 percent
**Licence:** Copernicus Sentinel data, free and open (CC-BY-like, attribution required: "contains modified Copernicus Sentinel data 2025")

### What the file is

| Property | Value |
|---|---|
| Format | Cloud-Optimized GeoTIFF |
| Size on the server | 305.6 MB |
| Dimensions | 10980 x 10980 pixels |
| Pixel size | 10 m |
| Bands | 3 (true colour composite, `uint8`) |
| CRS | EPSG:32630 (UTM zone 30N) |
| Internal tiling | 1024 x 1024 blocks |
| Overviews | levels 2, 4, 8, 16 |
| Extent (EPSG:32630) | 399960, 3990240 to 509760, 4100040 |

`TCI` means True Colour Image: the red, green and blue bands already stretched to 8 bit for viewing. Sentinel-2 has 13 bands; this asset is the human-readable three.

### Why this scene

MGRS tile 30SVF covers the Granada coast. Punta de la Mona sits at EPSG:32630 easting 434950, northing 4064947, comfortably inside the scene. Zero cloud, high summer, so the water is legible.

### The proof it is cloud-optimized

```
HEAD  ->  Content-Length: 305617967
          Accept-Ranges: bytes
GET   ->  Range: bytes=0-32767
          HTTP 206 Partial Content, 32768 bytes returned
```

A server that answers 206 to a range request, plus a file whose header is at the front and whose pixels are in tiles with overviews, is the whole COG idea. Nothing else.

---

## The vector layer on top

Reused from Module 3, no new download:
`../../03-vector-mpa/data/` (see the SOURCE.md there; the zip itself is not in the repo)

Unzip it once locally for QGIS. The polygon layer inside the geodatabase is `WDPA_WDOECM_poly_Sep2026_555523929`, 1 feature, EPSG:4326.

Note the CRS mismatch: raster in EPSG:32630, vector in EPSG:4326. QGIS reprojects on the fly for **display**. Module 3's rule still holds for **measurement**: to compute anything, reproject deliberately.

---

## Still to come

**Copernicus Marine SST**, deferred to session 3 or Module 10. Requires a free Copernicus Marine account and arrives as NetCDF, not GeoTIFF. Recorded here so it does not get quietly dropped.
