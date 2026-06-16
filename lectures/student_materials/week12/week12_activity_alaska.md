# Week 12 Activity: Working with Spatial Data in R

**BIO/CHEM 806 — Spatial Data**  
**Grading:** Completion-based  
**Submission:** Upload your rendered HTML and your `.qmd` source file

---

## Overview

This activity follows the spatial data workflow in [Textbook s15](https://nceas-learning-hub.github.io/sfsu_data_sci/s15_r_geospatial_vector_analysis.html). You will work with shapefiles and population data for Alaska to build up to a map that looks like this:

![Target map: Total Population by Alaska Region](images/r_geospatial_vector_analysis/alaska_population.png)

The textbook is your **primary reference** throughout — you are not expected to have the functions memorized. The goal is to practice reading documentation, following a reproducible workflow, and making your spatial decisions visible.

**Monday cohort:** Complete this as homework before Thursday.  
**Thursday cohort:** Work through this in-person with Nicole.

---

## Learning Objectives

- Read a shapefile into R using `read_sf()` and explore its structure
- Check and transform CRS with `st_crs()` and `st_transform()`
- Use `sf` objects with tidyverse functions (`select()`, `filter()`)
- Convert a CSV with lat/lon columns to an `sf` object with `st_as_sf()`
- Perform a spatial join with `st_join()`
- Aggregate spatially joined data with `st_drop_geometry()` + `group_by()` + `summarize()`
- Produce a multi-layer static map with `ggplot2` + `geom_sf()`
- Add a basemap with `ggspatial`
- Build an interactive map with `leaflet`
- Document spatial decisions in your decision log

---

## Setup

### Get the data

Download the Alaska shapefile data from KNB. You can do this programmatically — run this code once in your Console (not in your Quarto file):

```r
knb_url <- "https://dev.nceas.ucsb.edu/knb/d1/mn/v2/object/urn%3Auuid%3Aaceaecb2-1ce0-4d41-a839-d3607d32bb58"

download.file(url = knb_url, destfile = "shapefile_demo_data.zip")

unzip("shapefile_demo_data.zip", exdir = "data")

file.remove("shapefile_demo_data.zip")
```

This places three files in your `data/` folder:
- `ak_regions_simp.shp` — Alaska regional boundaries (shapefile)
- `alaska_population.csv` — community locations and population by city
- `ak_rivers_simp.shp` — Alaska rivers (shapefile)

### Create your Quarto file

Create a new Quarto file, title it **"Working with Spatial Data in R"**, and save it as `intro-to-spatial-data.qmd`.

### Load packages

```r
library(readr)
library(here)
library(sf)
library(ggplot2)
library(leaflet)
library(scales)
library(ggspatial)
library(dplyr)
```

---

## Part 1: Explore the Alaska Regions Shapefile

**1.1** Read in the Alaska regions shapefile using `read_sf()`. Add a `_sf` suffix to your object name.

**1.2** Make a quick plot with `plot()`. Note what you see — does anything look off?

**1.3** Check the class with `class()`. What two classes does the object have?

**1.4** Use `head()` and `glimpse()` to explore the object. Identify the `geometry` column. What geometry type is it?

**1.5** Check the CRS with `st_crs()`. What CRS is it in? Note the EPSG code in a comment.

**1.6** Transform to Alaska Albers (EPSG 3338) using `st_transform()`. Save with a `_3338` suffix. Plot again — what changed?

---

## Part 2: Practice with `sf` + Tidyverse

**2.1** Use `select()` to keep only the `region` column. What happens to the geometry column?

**2.2** Use `unique()` to see all region names, then `filter()` to keep only the `"Southeast"` region.

These should feel familiar — the key point is that geometry is **sticky** even when you `select()` other columns away.

---

## Part 3: Spatial Join — Population by Region

The main exercise from the textbook. You have population data by *city*; you need to assign each city to its *region*, then aggregate. Follow these steps:

**3.1** Read in `alaska_population.csv` with `read_csv()`. Use a `_df` suffix.

**3.2** Convert the data frame to an `sf` object using `st_as_sf()`:
- Coordinates are in columns `lng` (x) and `lat` (y)
- CRS is WGS84 (EPSG 4326)
- Use `remove = FALSE` to keep the raw lat/lng columns

Use a `_4326` suffix on the new object name.

**3.3** Try the spatial join directly — run `st_join()` with `join = st_within` to join your population points to the Alaska regions. What error do you get, and why?

**3.4** Fix the CRS mismatch with `st_transform()`, then redo the join. Use `head()` to inspect the result — what new column(s) came from the regions data?

**3.5** Use `st_drop_geometry()`, `group_by()`, and `summarize()` to calculate total population per region. Save as a plain data frame with a `_df` suffix.

**3.6** Use `left_join()` to attach the totals back to the Alaska regions shapefile. Quick-check your result with `plot()`.

**3.7** Save your joined spatial object to disk with `write_sf()`.

> 📖 The textbook walks through each of these steps in detail — refer to the **Spatial Joins** section.

---

## Part 4: Static Map with `ggplot2`

**4.1** Make a choropleth map of total population by region using `geom_sf()`. Use:
- `aes(fill = total_pop)` on the regions layer
- `scale_fill_continuous(low = "khaki", high = "firebrick", labels = comma)`
- `theme_bw()`
- A meaningful `labs()` title and legend label

**4.2** Now read in the rivers shapefile (`ak_rivers_simp.shp`). Check its CRS. If no EPSG is set explicitly, use [epsg.io](https://epsg.io) to figure out what it is.

**4.3** Build a three-layer map combining:
- Regional polygons filled by total population
- City points (your population `sf` object, transformed to 3338)
- Rivers with `aes(linewidth = StrOrder)` and `scale_linewidth(range = c(0.05, 0.5), guide = "none")`

**4.4** Save the map to your `figures/` folder with `ggsave()`.

> 📖 See the **Visualize with `ggplot`** section of the textbook.

---

## Part 5: Basemap with `ggspatial` (optional stretch)

**5.1** Transform your city points to EPSG 3857 (Pseudo-Mercator).

**5.2** Add `ggspatial::annotation_map_tile(type = "osm", zoom = 4, progress = "none")` as the first layer in a new `ggplot()` call, then add your city points colored by population.

**5.3** Does the basemap add value here, or does it clutter the map? Note your thinking for Part 6.

> ⚠️ This step can take a few minutes to load tiles. If it times out, skip it and note that in your decision log.

> 📖 See the **Incorporate base maps into static maps using `ggspatial`** section.

---

## Part 6: Interactive Map with `leaflet`

Build the interactive map from the textbook. This requires:

**6.1** Define the Alaska Albers CRS for leaflet using `leaflet::leafletCRS()` — copy the code from the textbook exactly.

**6.2** Transform your regions data back to WGS84 (EPSG 4326) for leaflet.

**6.3** Build the map in stages:
- First: polygons only, filled gray, to confirm it's working
- Then: add a color scale (`colorNumeric()`), fill polygons by `total_pop`, add region labels
- Finally: add circle markers for individual cities, sized by `log(population / 500)`, with popup labels

**6.4** Add a legend with `addLegend()`.

> 📖 See the **Visualize `sf` objects with `leaflet`** section. The textbook builds this map up in three stages — follow along.

---

## Part 7: Decision Log Entry

Add an entry to your `decision_log.qmd` documenting at least **two** decisions you made. Good candidates:

- Which CRS did you choose for the spatial join — and why is Alaska Albers (EPSG 3338) a good choice for Alaska data?
- What join type did you use — and what would `st_intersects` have given you instead of `st_within`?
- What does `do_union` do in `group_by()` + `summarize()` on an `sf` object — and when would you set it to `FALSE`?
- Did you add the basemap in Part 5? Why or why not?
- What can you see in the `leaflet` map that the static map can't show?

---

## Submission

Upload to Canvas:
1. Your rendered **HTML file** (`intro-to-spatial-data.html`)
2. Your **`.qmd` source file** (`intro-to-spatial-data.qmd`)
3. Your updated **`decision_log.html`**

---

## Grading

Graded on **completion**. Full credit for a genuine attempt at all parts through Part 6 (Part 5 is optional stretch). If you get stuck, note where and what you tried — that counts.

---

## Hints and Common Pitfalls

**CRS mismatch error:**
```
Error: st_crs(x) == st_crs(y) is not TRUE
```
→ This is expected in Part 3.3 — it's a teaching moment. Transform with `st_transform()` and redo the join.

**Rivers CRS has no EPSG code set:**  
→ Look at the proj4 string from `st_crs()` and search [epsg.io](https://epsg.io) to identify it. Textbook hint: it is EPSG 3338.

**`leaflet` shows a blank map:**  
→ Confirm your data is in EPSG 4326 before passing to `leaflet`. The custom `epsg3338` CRS object also needs to be defined first.

**`comma` labels not working:**  
→ Make sure `library(scales)` is loaded — `comma` is from the `scales` package.

---

## Resources

- 📖 [Textbook s15 — Working with Spatial Data](https://nceas-learning-hub.github.io/sfsu_data_sci/s15_r_geospatial_vector_analysis.html)
- `?st_join` — lists all available spatial join types
- [epsg.io](https://epsg.io) — look up EPSG codes
- [Leaflet for R](https://rstudio.github.io/leaflet/)
