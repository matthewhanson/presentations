<div>How cloudy is London really?</div>
<hr />
<div>
    <div style="text-align: right; font-size: 80%">
        Matthew Hanson<br />
        @geoskeptic
    </div>
</div>

---

## Introduction

- New Hampshire - Live Free or Die!
- 2 teenage daughters
- Like snow
- circa 1985
  - Photography > Imaging Science > Remote sensing
- Open-source geospatial software

----

### Recent work

- Cumulus: Dockerization of algorithms
- Land cover classification and mapping: Malawi, Ethiopia, Kenya
- Monitoring special economic zones with Digital Globe imagery and VIIRS nightlights
- MODIS Public dataset
- Contributor to development of STAC specification
- sat-utils: OS tools for search and access of imagery
- Landsat and Sentinel catalogs
- Coastline mapping and conflation (Landsat, Sentinel, Planet)

---

## The Project

- Pick NASA Earthdata dataset (of 5k!) and tell a story
- Initial ideas
    - Do something different than imagery
    - Arctic ground samples measuring permafrost
    - LiDAR and birds
- Most datasets are single campaign in support specific science research
    - I wanted to tell a story over time
- So, back to imagery

----

### MODIS

- I :heart: MODIS
- Science!
- It really is a gold standard: [MCD43A4](https://lpdaac.usgs.gov/dataset_discovery/modis/modis_products_table/mcd43a4_v006)
- [MODIS Land products](https://lpdaac.usgs.gov/dataset_discovery/modis/modis_products_table)
- [MODIS Atmosphere products](https://modis-atmos.gsfc.nasa.gov/products/)

----

### Remote Sensing

-----
Adage #2

Remote Sensing loves The Cloud

-----
Adage #3

Remote Sensing hates clouds

-----

---

### Clouds: An eternal problem

- Cloud detection algorithms
    - range from terrible to not-that-great
    - Fmask, ACCA for Landsat
    - Sentinel-2 masks terrible (no thermal bands)
- 50% cloud cover does NOT = 50% usable pixels
    - Shadows, Point Spread Function, haze/mist, or otherwise compromised pixels
    - 50% cloud cover often = 0% usable pixels

----

### Handling clouds

- The "Cloud-Free Image" Approach
- Time series, using all valid pixels


----

### Cloud questions

1. Relative cloudiness
    - How cloudy is this place compared to that place?
2. When am I more likely to get a cloud-free image?
    - What seasons?
    - When should I task a satellite, or fly a drone?
3. How many valid observations can I expect over time?
    - I know temporal frequency of my sat
    - For a pixel, how many observations can I expect?

-----

<div class="fragment" style="font-size: 70%">
Spoiler:
- #1 has been answered by others, I didn't answer #2 or #3
</div>

----

### Global Cloud Map

- [Global map of average cloud cover 2002-2015](https://www.vox.com/2015/5/11/8585439/cloud-map-global)
- [Images of monthly averages](http://eclipsophile.com/global-cloud-cover/)
- [Monthly averages and variability over 15 years](https://www.earthenv.org/cloud)

-----

- Above all based on MODIS
- Average as a metric isn't whole story
- But how often I can get a cloud-free image?

----

### Cloud Products

[MODIS Standard Atmosphere Products](https://modis-atmosphere.gsfc.nasa.gov/products)

- MOD06_L2 - 250 subdatasets of cloud metrics
- MOD35_L2 - masks of cloud types

- Derived cloud masks from Land products?
    - Cloud cover properites of MODIS daily images?
    - QC band for MCD products, using 16 day window?

---

## Python Backend Library

- Query CMR for MOD35 scenes within `geometry` from `start_date` to `end_date`
- Download HDF file
- Extract Cloud Mask subdataset from HDF
- Reproject to lat/lon
- Extract cloud mask from bitmask, save as COG
- Clip byte mask by AOI
- Calculate cloud percentage as
    - (# of cloudy pixels / # of valid pixels)
    - Valid pixels from nodata mask

----

### Command Line Interface (CLI)

```
usage: modis_clouds.py [-h] [--version] [--log LOG] [--start START]
                       [--end END] [--datadir DATADIR] [--process] [--vis]
                       [--night]
                       intersects

MODIS Cloud Processing (v0.1.0)

positional arguments:
  intersects         GeoJSON Feature to search against

optional arguments:
  -h, --help         show this help message and exit
  --version          Print version and exit
  --log LOG          0:all, 1:debug, 2:info, 3:warning, 4:error, 5:critical
                     (default: 2)
  --start START      Starting date (default: 2019-01-01)
  --end END          Ending date (default: 2019-02-27)
  --datadir DATADIR  Save to this directory (default: data)
  --process          Convert cloud mask to COG (default: False)
  --vis              Download visible daily imagery too (default: False)
  --night            Also download night scenes (default: False)
```

----

### Sanity checks with QGIS

- Reprojection of HDF subdatasets
    - Atmosphere products use geolocation arrays
- Does the cloud mask match up with optical scenes?

---

### Analysis

- Jupyter notebook
- Read CSV data from backend, explore with plots

----

### Poor assumptions

- Assumes pixel area doesn't vary much over an AOI
    - using lat/lon projection
    - Not equal area
    - Reproject to an equal area projection using center lat/lon of AOI
        - Or UTM, which is close enough to equal area
- Compare AOIs of equal areas
- Ignores incomplete AOIs (AOI on tile edge)

----

### Further directions and questions

- Likelihoods, not averages
- Nighttime vs Daytime
    - Algorithms are different
    - How do cloud masks compare, on average?
    - Over or underestimated during day or night?
- MODIS Aqua!
- VIIRS!!
- Look at high probability clouds only
- Examine seasonal variations using Fourier Analysis
- Predictive model
    - Predictive cloud cover for any given day of year

----

### Questions ?

---