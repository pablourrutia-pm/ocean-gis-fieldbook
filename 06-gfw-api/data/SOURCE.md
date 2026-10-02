# Data sources, notebook 6

No data files are stored here. Everything is fetched live when the notebook runs.

## Global Fishing Watch API

- **What:** vessel identity search (Vessels API) and apparent fishing effort (4Wings report, `public-global-fishing-effort`), 2025, monthly, 0.01 degree cells, grouped by flag.
- **Area:** box lon -4.1 to -3.3, lat 36.55 to 36.80 (Granada coast, coast to about 25 km offshore).
- **Access:** free GFW API token (globalfishingwatch.org/our-apis/tokens), stored in Colab Secrets as `GFW_API_ACCESS_TOKEN`. Never in the notebook.
- **Client:** `gfw-api-python-client` 1.4.0.
- **Terms:** non-commercial use, attribution to Global Fishing Watch required. See the GFW API terms of use.

## Open-Meteo Marine API

- **What:** daily maximum wave height, 2025, at 36.65 N, 3.72 W (about 8 km off Punta de la Mona).
- **Access:** no key needed for non-commercial use.
- **Licence:** CC BY 4.0, attribution to Open-Meteo.

## WDPA (Protected Planet)

- Same file as notebook 3: `WDPA_WDOECM_Sep2026_Public_555523929.zip`. Upload it to Colab before running the map cells. Non-commercial, citation required, not redistributed here.
