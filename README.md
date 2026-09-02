# Hydro Quality Clustering — Complex Data course (M1 SDSC)

Project on tracking French river water quality using the NAIADES dataset.
The brief: *"define quality classes for monitoring stations in relation to their geographic location."*

> **Hey Kian.** You don't need to rerun everything to remember what we did — it's all already computed below.

## Quick read (recommended)

Just open these 3 notebooks, outputs already included:

1. `outputs/TP1_etude_preliminaire_executed.ipynb`
2. `outputs/TP2_agregation_clustering_executed.ipynb`
3. `outputs/TP3_visualisation_geo_executed.ipynb`

These are from a full clean run, top to bottom, no errors. No need to fire up Python or download Corine Land Cover again — stats, clustering, maps, it's all there.

## If you want to rerun everything yourself

Open the notebooks in `notebooks/` and run them in order:

1. `notebooks/TP1_etude_preliminaire.ipynb` → produces `outputs/tables/tp1_clean_dataset.csv`
2. `notebooks/TP2_agregation_clustering.ipynb` → produces `outputs/tables/tp2_station_clusters.csv`
3. `notebooks/TP3_visualisation_geo.ipynb` → produces the maps and cross-tabs

You'll need Python 3.12 with `pandas numpy matplotlib seaborn scikit-learn geopandas shapely zstandard pyogrio`, plus the raw data files that aren't in the repo (too big):
- `data/raw/analyses_2022.csv.zst` (~101 MB)
- `data/raw/stations.csv.zst`
- `data/raw/operations_2022.csv.zst`
- `data/geo/CLC_PNE_RG/CORINE_LAND_COVER_FRANCE_METROPOLITAINE_EPSG2154.gpkg` (~1.26 GB) — link in `data/reference/corine-land-cover.txt`

## Where the results live

```
outputs/
├── TP1_etude_preliminaire_executed.ipynb     TP1 notebook with outputs
├── TP2_agregation_clustering_executed.ipynb  TP2 notebook with outputs
├── TP3_visualisation_geo_executed.ipynb      TP3 notebook with outputs
├── tables/
│   ├── tp1_clean_dataset.csv                 cleaned data (19,349 × 18)
│   ├── tp2_station_clusters.csv              clusters per station (2,936 × 3)
│   ├── tp3_crosstab_cluster_clc_effectifs.csv
│   └── tp3_crosstab_cluster_clc_pct.csv
└── figures/
    ├── carte_clusters_p95_clc_niv2.png       main map (clusters over CLC level 2)
    ├── carte_clusters_mean_clc_niv2.png      variant (cluster_mean)
    ├── heatmap_cluster_clc.png               cluster × land cover heatmap
    └── stations_clusters_nofond.png          quick preview, no basemap
```

## Making sense of the main outputs

**`tp1_clean_dataset.csv`** — Wide table of station × sample × date, one column per physico-chemical parameter. We kept the **15 most frequent parameters** and only samples where all 15 were measured together (plain `dropna`). This is the clean entry point for TP2.

**`tp2_station_clusters.csv`** — Two cluster labels per station, KMeans with **k = 5** (matching the 5 SANDRE/SEQ-Eau classes):
- `cluster_mean`: annual mean aggregation of the raw (normalized) values.
- `cluster_p95`: 95th-percentile aggregation over the **SEQ-Eau classes** — closer to how it's actually done in practice, since it surfaces the "bad" values instead of averaging them away.

TP3 uses `cluster_p95` as the main classification — it's more balanced and easier to interpret.

**`tp3_crosstab_cluster_clc_*.csv`** — Crosses the clusters with Corine Land Cover **level 2** land-use categories (codes 11, 21, 23, 24, 31, ...). This is what answers the actual question from the brief: do water-quality clusters relate to land use? Short answer: **yes** — cluster 0 skews more forested, cluster 4 more dominated by arable land.

**Maps (`outputs/figures/`)** — Stations colored by cluster, overlaid on CLC level-2 polygons (official 14-category legend).

## Full structure

```
.
├── README.md                          this file
├── notebooks/                         notebooks to run
│   ├── TP1_etude_preliminaire.ipynb
│   ├── TP2_agregation_clustering.ipynb
│   └── TP3_visualisation_geo.ipynb
├── outputs/                           outputs + executed notebooks
├── data/
│   ├── raw/        NAIADES data (not versioned, see above)
│   ├── reference/  assignment brief, SEQ-Eau grids, SANDRE class table
│   └── geo/        Corine Land Cover (not versioned)
└── archive/                           original professor-provided solutions
```

## Notes for the graded TP4

- The pipeline needs to work on a **different dataset** (probably `data/raw/naiades-RM.zip`, the Rhin-Meuse set).
- Keep the same methodology: 15 most frequent parameters, 95th-percentile aggregation on SEQ-Eau classes, KMeans k=5.
- Things to discuss in the report: mean vs. P95, limits of the CLC crossing (point polygon vs. riparian buffer), and next steps (BD Carthage, upstream watershed).

Good luck — you've got this.
