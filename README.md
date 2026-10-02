# Ocean GIS fieldbook

Hands-on GIS, Python and Earth observation work on one marine protected area on the Granada coast. It is the groundwork for a public product built on Global Fishing Watch APIs.

I am a product manager with six years on GIS and sustainability data products. This repo is where I build enough hands-on fluency to scope technical work with the engineers who do it, and to prototype with real data myself. For too long this has been a lacking skill for me and its time to solve it.

## One site, four ways of seeing it

Every notebook uses the same place: **Acantilados y Fondos Marinos de La Punta de La Mona**, a marine Special Area of Conservation (WDPA id 555523929).

1. **As a dive spot.** A coordinate I typed by hand. (notebook 1)
2. **As an official boundary.** The polygon from the World Database on Protected Areas. (notebook 3)
3. **As pixels from space.** The same boundary on a Sentinel-2 image that never gets downloaded. (notebook 4)
4. **As fishing pressure.** A year of apparent fishing effort around it, from the Global Fishing Watch API. (notebook 6)

## What is here

| Folder | What it shows | The number that matters |
|---|---|---|
| [`01-dive-spots-folium/`](01-dive-spots-folium/) | Python basics ending in an interactive folium web map of five dive sites | 5 sites, depths 10 to 49 m |
| [`03-vector-mpa/`](03-vector-mpa/) | Loading the MPA boundary from an Esri File Geodatabase with GeoPandas, and why the CRS matters for measuring | 1.25 km², against 0.000126 "square degrees" in EPSG:4326, which is not an area |
| [`04-raster-cog/`](04-raster-cog/) | Reading a Sentinel-2 Cloud-Optimized GeoTIFF over HTTP, plus a QGIS map of the MPA on top | 305.6 MB on the server, 32 KB moved |
| [`06-gfw-api/`](06-gfw-api/) | First calls to the Global Fishing Watch API: vessel identity, a year of fishing effort on the Granada coast, a monthly pattern tested against closures and sea state, and the effort mapped around the reserve | 23,183 apparent fishing hours in 2025; 0.66% of them within 1 km of the reserve |
| [`formats-in-my-own-words.md`](formats-in-my-own-words.md) | Vector and raster formats explained for a colleague who is not a GIS person | |

Notebook 2 (pandas) was never saved. Its work (filter, group, plot, answer one question) now lives in notebook 6, on Global Fishing Watch API data instead of a CSV download.

## Things I learned that change how I would scope work

- **Reported is not measured.** WDPA lists this site's reported area as 0 km². The geometry says 1.25 km². When you quote a number, know which column it came from.
- **Degrees are not metres.** An area computed in EPSG:4326 is meaningless. You have to reproject to a metric CRS (here EPSG:25830) before you measure anything.
- **Format decides cost.** A COG lets a client fetch only the tiles it needs. Whether a dataset is cloud-optimized decides whether serving it is cheap or expensive.
- **One MMSI is not one vessel.** Searching one transponder number returned five GFW vessel identities, only one matching official registries. Any feature that counts vessels must say which identity it uses.
- **AIS sees the big boats.** EU rules require AIS on fishing vessels of 15 m or more. The small inshore boats that fish nearest this reserve are largely invisible, so effort near the coast is undercounted.
- **The reserve is smaller than the data.** Fishing effort comes in cells of about 1 km; the reserve is 1.25 km². The product has to work at coast scale with the reserve as context, not as the unit of analysis.
- **Harbours look like fishing grounds.** The hottest cells on the map sit on Caleta de Vélez, Nerja and Motril: slow manoeuvring in port read as fishing. The API's `distance_from_port_km` filter is the fix.
- **Basemaps have terms.** OpenStreetMap's volunteer tile servers block requests from Colab under their usage policy. Choosing a tile provider is a scoping decision with a policy and sometimes a cost, not a styling choice.

## Data and licences

No data files are stored in this repo. Each `data/SOURCE.md` says where the data comes from, how to fetch it again, and under what licence.

- WDPA: Protected Planet, non-commercial use, citation required.
- Sentinel-2: Copernicus, free and open. Contains modified Copernicus Sentinel data 2025.
- Basemap in notebook 1: Esri World Imagery.
- Global Fishing Watch API: non-commercial use, attribution required.
- Open-Meteo Marine API: CC BY 4.0.

Code in this repo is under the MIT licence.

## How to run

Each notebook runs in Google Colab. Notebook 3 needs the WDPA zip downloaded first (see its `data/SOURCE.md`). Notebook 4 needs nothing local, because the raster is read straight from its bucket. Notebook 6 needs a free GFW API token saved in Colab Secrets as `GFW_API_ACCESS_TOKEN`, and the WDPA zip for the map cells.

## Next

- **Next:** a separate product repo with a scoping brief, an architecture sketch and a decision log, and a web map deployed on GitHub Pages.

## Log

| Date | Ship |
|---|---|
| 2026-10-01 | Repo public: notebooks 1, 3 and 4, the formats note, MIT licence |
| 2026-10-02 | Notebook 6: first Global Fishing Watch API calls, fishing effort mapped around the reserve |
