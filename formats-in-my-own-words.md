# Formats explained in my own words

*Module 3 artifact (vector half). Module 4 adds the raster half to this same file. Written for a colleague who is not a GIS person, in my words, not copied.*

---

## Vector

### Point, line, polygon

A point marks a singular, specific spot, like the exact location of a unique coral head I found during a dive. A line defines a path or connection between points, such as the route I swam to survey a section of reef. A polygon represents an enclosed area, like the outline of a marine protected area I'm studying.

Worth adding: which of the three a dataset uses is a modelling decision tied to the scale it was built for, not a fact about the real object. The same reef is a point on a regional map and a polygon on a site map.

### GeoJSON

GeoJSON is a human-readable text format that describes geographical shapes and their properties, making it super easy to share map data, especially online.

The tradeoff: readable text means large files and slow loads, so big datasets move to vector tiles, GeoParquet or FlatGeobuf.

### CRS and why degrees are not metres

Measuring area in EPSG:4326 is inaccurate because it uses latitude and longitude degrees, which don't represent uniform distances across the curved Earth, so calculations will be distorted.

Concretely: a degree of longitude is about 111 km at the equator and about 85 km at 40 degrees north (Almeria), while a degree of latitude stays roughly 111 km. A square degree is therefore not a unit of area. To measure, reproject to a projected CRS in metres, for example EPSG:25830 (UTM zone 30N) for Almeria.

---

## Raster

### What a raster is

A raster is a grid of equal cells, each holding one number, pinned to the Earth by four facts: where the top-left corner sits, how big a cell is, how many rows and columns, and in which CRS. No rows, no names, no attributes.

The test that separates it from vector: **click a vector feature and you get a row** (NAME, DESIG_ENG, STATUS_YR). **Click a raster cell and you get a number.**

A raster is defined by its grid, not by its job. It is not "the layer underneath" and not "the picture you export". Fishing effort is a raster because GFW bins the ocean into squares and each square holds hours of fishing, a value that exists everywhere including zero. Dive spots are vector points, even when drawn as dots.

Same phenomenon, two models, and which one you get is a decision someone made: a fire detection is a point (vector), a burned-area or canopy-cover map is a grid (raster).

### Two kinds of raster, worth never confusing

- A raster of **measurements**: each cell is degrees C, metres of depth, mg/m3 of chlorophyll, hours of fishing. Analysable.
- A raster of **pixels**: each cell is a colour. A satellite true-colour image, or the PNG exported at the end of a QGIS session. A picture of data, not data.

Both are grids. Only the first can be queried, thresholded or averaged. "We have the raster" is ambiguous until you know which one is meant.

### Resolution

Metres per cell. It is a promise about the smallest thing you can see, not about accuracy. 10 m pixels means a 10 m boat is one pixel at best. A raster can be beautifully precise and completely wrong.

### GeoTIFF vs COG

A GeoTIFF is an image with a location for it, like a satellite image: this is an image of this place, you could put it here. A COG is the same thing but with the bytes ordered and indexed, so you can ask for a specific part of that image without loading it all.

Two halves to add:

1. **The overviews.** A COG also contains pre-made smaller copies of itself, inside the same file. Reading a part cheaply is half the win. The other half is that zooming out reads a small copy instead of shrinking 10980 x 10980 pixels.
2. **The server has to cooperate.** The client asks for bytes 0 to 32767 and the server must answer `206 Partial Content`. A perfectly organised COG hosted on a server with no range support behaves like an ordinary GeoTIFF. So "is it a COG" is two questions, one about the file and one about where it lives.

The version to use in a scoping call is an architecture claim, not a file claim: a COG lets you serve rasters out of a plain bucket, with no database and no pre-render pipeline. Migrating to COG is a re-tiling job, not a data conversion.

### Map tiles

Imagine a chess board where each square takes three seconds to load. Loading the whole board every time you want to look at one area takes ages. Asking for 3F takes three seconds. Serving a map as tiles, and knowing how they are arranged, means fewer requests, less data travelling and faster rendering.

The half that makes it work at every scale: tiles are a **pyramid**, not one grid. Zoom 0 is a single tile for the whole world, and each level splits every tile into four. Every tile stays 256 x 256 pixels, so detail quadruples per zoom level while the download per screen stays flat. **That flat cost per screen is why zooming in does not get more expensive**, and it is the trick that made web maps possible.

Two more worth keeping:

- The address is `/{z}/{x}/{y}.png`. Three integers, and that is the entire API of most basemaps. Web Mercator (EPSG:3857) is used not because it is a good projection, it inflates Greenland grotesquely, but because it makes the world a square that halves cleanly forever. A projection chosen to fit a data structure.
- **Raster tiles** are finished pictures: styling is baked in, and changing a colour means re-rendering every tile. **Vector tiles** ship the geometry and the browser draws it, so you restyle instantly and can click individual features. This is why MapLibre exists.

And the link back to COG: with a COG plus a dynamic tile server, a tile request becomes a couple of range requests against the original file, cut on the fly. No pre-render, no tile store, no pipeline to keep in sync. A whole subsystem removed. Knowing why it disappears is worth more than knowing its name.
