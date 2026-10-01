# Data source note

**File:** `WDPA_WDOECM_Sep2026_Public_555523929.zip`
**Downloaded:** 2026-09-10
**Source:** Protected Planet (WDPA), https://www.protectedplanet.net/555523929?site_pid=555523929
**Release:** WDPA September 2026 public release
**Licence:** WDPA terms of use, non-commercial, citation required. See the PDFs inside the zip.

## What is in it

- `WDPA_WDOECM_Sep2026_Public_555523929.gdb/` : an Esri File Geodatabase, a folder of binary files, not a shapefile and not a GeoJSON.
  - layer `WDPA_WDOECM_poly_Sep2026_555523929` : the boundary, 1 feature, MultiPolygon, EPSG:4326
  - layer `WDPA_WDOECM_source_Sep2026_555523929` : a non-spatial metadata table saying where the boundary came from
- Manuals and attribute tables as PDFs in five languages. Ignore for now, useful when decoding attribute codes.

## The site

- **Name:** Acantilados y Fondos Marinos de La Punta de La Mona
- **WDPA site id:** 555523929
- **Designation:** Special Area of Conservation (EU Habitats Directive), regional, designated 2015
- **Realm:** Marine
- **Reported area:** 0.0 km2 (REP_AREA not filled in by the reporting authority)
- **GIS area:** 1.25 km2 total, 1.17 km2 marine

Note the gap between REP_AREA and GIS_AREA. Reported figures in WDPA are frequently zero or stale, the GIS figures are computed from the geometry. Knowing which column you are quoting is the kind of detail that separates a scoped question from a wrong number in a slide.

## Why the file is not in this repo

Two reasons. The WDPA terms of use restrict redistribution, so the right move is to point at the source, not re-host it. And git keeps every version of every file forever, so binary data permanently bloats a repo. To run the notebook, download the zip from the link above and place it in this `data/` folder.
