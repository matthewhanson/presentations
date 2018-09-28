<div>Finding and using public earth imagery with sat-utils</div>
<hr />
<div>
    <img src='ds-logo.png' style="border:none; height:30%; width:30%; box-shadow:none;" />
    <div style="text-align: right; font-size: 80%">
        Matthew Hanson<br />
        @geoskeptic
    </div>
</div>

---

# Workshop Overview 
- Overview of public imagery 
- What is STAC? 
- Searching image metadata 
- Downloading imagery 
- Applications

---

# Earth on AWS https://aws.amazon.com/earth/ 
- Geospatial data stored on AWS S3 
- Open to everyone 
- Copies, not original source 
- Often converted into more friendly formats

----

# Landsat 
- Landsat program operating since 1972 
- Currently Landsat-7 and Landsat-8 are operational 
- Only Landsat-8 is on AWS 
- Launched in 2013, giving us a 5 year archive 

------ 

##### Landsat-8 
- Spectral bands 
- RGB, Near-Infrared, 2 Short-Wave Infrared: 30 meters - Panchromatic: 15 meters - Revisit Rate: 16 days

# Sentinel-2 
- Sentinel-2 sensors 
- Sentinel-2A launched mid-2015 
- Sentinel-2B launched mid-2017 
- Revisit rate 
- 10 days each (every 5 days using 2A and 2B) Spectral Bands 
- 13 total bands 
- RGB, NIR: 10 meters 
- 2 Short-wave IR, Red Edge: 20 meters

# The State of satellites https://landscape.satsummit.io/ 
- Overview of current satellites specifications 
- Example imagery

# Metadata first 
- With growth of satellites and sensors, how do we catalog imagery? 
- How do we search data from different providers with different metadata? STAC 
- a new spec standard for data providers to describe their imagery

# What is STAC?
Spatio-Temporal Asset Catalog - metadata
<br /> 
set of standard fields for geospatial data - catalog<br />
linked flat files that can be crawled - api<br />
REST API for querying geospatial data https://github.com/radiantearth/stac-spec/ *note*: rapidly changing

# A common core: "Where and When" 

| element | description | |-----------------|-----------------| | id | Provider ID for the item | | geometry | Bounding Box + Footprint of the item in lat/long (EPSG 4326) | | datetime | The searchable date/time of the assets, in UTC (Formatted in RFC 3339) | | provider | Provider name | | license | Item's license name based on SPDX License List or following guidelines for non-SPDX licenses | | links | Array of link objects to resources and related URLs (self required) | | assets | Dictionary of asset objects that can be be download |

# Electro-Optical (EO) extension

| element | description | |----------------------|---------------------------| | eo:gsd | The minimum distance between pixel centers available, in meters | | eo:platform | Specific name of the platform (e.g., landsat-8, sentinel-2A, larrysdrone) | | eo:instrument | Name of instrument or sensor (e.g., MODIS, ASTER, OLI, Canon F-1) | | eo:epsg | EPSG code of the data SRS, null if no crs | eo:cloud_cover | Percent of cloud cover (1-100) | | eo:off_nadir | Viewing angle. 0-90 degrees, measured from nadir | eo:azimuth | Viewing azimuth angle. 0-360 degrees, measured clockwise from north | eo:sun_azimuth | Sun azimuth angle. 0-360 degrees, measured clockwise from north | eo:sun_elevation | Sun elevation angle. 0-90 degrees measured from horizon | eo:bands | Band specific metadata (see below)

#### assets and eo:bands
```json
"eo:bands": { "B1": { "common_name": "blue", "gsd": 30, "center_wavelength": 0.48, "full_width_half_max": 0.06 }, "B2": { "common_name": "green", "gsd": 30, "center_wavelength": 0.56, "full_width_half_max": 0.06 }, "B3": { "common_name": "red", "gsd": 30, "center_wavelength": 0.65, "full_width_half_max": 0.04 }, "B4": { "common_name": "pan", "gsd": 15, "center_wavelength": 0.59, "full_width_half_max": 0.18 }, 
```
```json
"assets": { "datafile": { "href": "url/to/datafile", "eo:bands": [ "B3", "B2", "B1" ] }, "panband": { "href": "url/to/panbandfile", "eo:bands": [ "B4" ] }
````

# Common Band Names | Common Name | Band Range (μm) | Landsat 5 | Landsat 7 | Landsat 8 | Sentinel 2 | MODIS | |----------------------|---------------------------|-------------------------|---------------------------------------------------------------------------------------------|------------------------------------|------------------------------------|------------------------------------| | coastal | 0.40 - 0.45 | | | 1 | 1 | |blue | 0.45 - 0.5 | 1 | 1 | 2 | 2 | 3 |green | 0.5 - 0.6 | 2 | 2 | 3 | 3 | 4 |red | 0.6 - 0.7 | 3 | 3 | 4 | 4 | 1 |pan | 0.5 - 0.7 | | 8 | 8 | | |nir | 0.77 - 1.00 | 4 | 4 | 5 | 8 | 2 |cirrus | 1.35 - 1.40 | | | 9 | 10 | 26 |swir16 | 1.55 - 1.75 | 5 | 5 | 6 | 11 | 6 |swir22 |2.1 - 2.3 | 7 | 7 | 7 | 12 | 7 |lwir11 | 10.5 - 11.5 | | | 10 | | 31 |lwir12 | 11.5 - 12.5 | | | 11 | | 32

# sat-api as a dynamic STAC API 
- STAC compliant 
- *warning*: rapidly changing spec - watch our blog at http://developmentseed.org 
- all landsat-8 and sentinel-2 scenes on AWS - 5.5 million records 
- returns a GeoJSON FeatureCollection - https://sat-api.developmentseed.org/search/stac

# Landsat example <section> <pre><code data-trim style="height:500px; max-height:500px"> { "type": "Feature", "properties": { "id": "LC08_L1TP_162032_20180308_20180320_01_T1", "collection": "landsat-8", "datetime": "2018-03-08T06:53:55.416Z", "eo:cloud_cover": 22, "eo:sun_azimuth": 149.76873357, "eo:sun_elevation": 40.21040562 }, "bbox": [ 55.10313, 39.67008, 57.83389, 40.97852 ], "geometry": { "type": "Polygon", "coordinates": [ [ [ 57.83389, 40.97852 ], [ 55.62095, 41.38693 ], [ 55.10313, 39.67008 ], [ 57.26098, 39.26382 ], [ 57.83389, 40.97852 ] ] ] }, "assets": {...}, "links": [ { "rel": "self", "href": "" }, { "rel": "index", "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/index.html" }, { "rel": "collection", "href": "https://sat-api.developmentseed.org//collections?id=landsat-8" } ] } </code></pre> </section>

# Collections - A set of multiple items with shared fields (e.g., Landsat-8, Sentinel-2)
<div style="font-size: 70%"> https://sat-api.developmentseed.org/collections?collection=landsat-8 </div>
<section> <pre><code data-trim style="height:600px; max-height:600px"> 
{ "type": "FeatureCollection", "properties": { "found": 1, "limit": 1, "page": 1 }, "features": [ { "type": "Feature", "properties": { "collection": "landsat-8", "description": "Landat 8 imagery radiometrically calibrated and orthorectified using gound points and Digital Elevation Model (DEM) data to correct relief displacement.", "provider": "USGS", "license": "PDDL-1.0", "eo:gsd": 30, "eo:platform": "landsat-8", "eo:instrument": "OLI_TIRS", "eo:off_nadir": 0, }, "links": [ { "ref": "self", "href": "https://sat-api.developmentseed.org/search/stac?id=landsat-8" } ], "eo:bands": { "B1": { "common_name": "coastal", "gsd": 30, "accuracy": null, "center_wavelength": 0.44, "full_width_half_max": 0.02 }, "B2": { "common_name": "blue", "gsd": 30, "accuracy": null, "center_wavelength": 0.48, "full_width_half_max": 0.06 }, "B3": { "common_name": "green", "gsd": 30, "accuracy": null, "center_wavelength": 0.56, "full_width_half_max": 0.06 }, "B4": { "common_name": "red", "gsd": 30, "accuracy": null, "center_wavelength": 0.65, "full_width_half_max": 0.04 }, "B5": { "common_name": "nir", "gsd": 30, "accuracy": null, "center_wavelength": 0.86, "full_width_half_max": 0.03 }, "B6": { "common_name": "swir16", "gsd": 30, "accuracy": null, "center_wavelength": 1.6, "full_width_half_max": 0.08 }, "B7": { "common_name": "swir22", "gsd": 30, "accuracy": null, "center_wavelength": 2.2, "full_width_half_max": 0.2 }, "B8": { "common_name": "pan", "gsd": 15, "accuracy": null, "center_wavelength": 0.59, "full_width_half_max": 0.18 }, "B9": { "common_name": "cirrus", "gsd": 30, "accuracy": null, "center_wavelength": 1.37, "full_width_half_max": 0.02 }, "B10": { "common_name": "lwir11", "gsd": 100, "accuracy": null, "center_wavelength": 10.9, "full_width_half_max": 0.8 }, "B11": { "common_name": "lwir12", "gsd": 100, "accuracy": null, "center_wavelength": 12, "full_width_half_max": 1 } } } ] } </code></pre> </section>

# Landsat Assets
```json
"assets": { "ANG": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_ANG.txt" }, "B1": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B1.TIF", "eo:bands": [ "B1" ] }, "B2": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B2.TIF", "eo:bands": [ "B2" ] }, "B3": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B3.TIF", "eo:bands": [ "B3" ] }, "B4": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B4.TIF", "eo:bands": [ "B4" ] }, "B5": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B5.TIF", "eo:bands": [ "B5" ] }, "B6": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B6.TIF", "eo:bands": [ "B6" ] }, "B7": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B7.TIF", "eo:bands": [ "B7" ] }, "B8": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B8.TIF", "eo:bands": [ "B8" ] }, "B9": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B9.TIF", "eo:bands": [ "B9" ] }, "B10": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B10.TIF", "eo:bands": [ "B10" ] }, "B11": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_B11.TIF", "eo:bands": [ "B11" ] }, "BQA": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_BQA.TIF" }, "MTL": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_MTL.txt" }, "thumbnail": { "href": "https://landsat-pds.s3.amazonaws.com/c1/L8/162/032/LC08_L1TP_162032_20180308_20180320_01_T1/LC08_L1TP_162032_20180308_20180320_01_T1_thumb_large.jpg" } }
```

# Using sat-api 
- Search on any property 
- Partial datetimes supported 
- Range searches on numerics using a / (forward slash)
Two pass search
1. search the collections - match on matching field OR missing field
2. search the items - match items within any collection from step 1 - match on matching fields OR missing field
<hr />

# Let's explore sat-api 
- Install JSONView browser extension Main search endpoint <div style="font-size: 70%"> https://sat-api.developmentseed.org/search/stac </div> Landsat-8 scenes from 2017 with 0-20% cloud cover <div style="font-size: 70%"> https://sat-api.developmentseed.org/search/stac?datetime=2017&collection=landsat-8-l1&eo:cloud_cover=0/20 </div> All scenes between Dec 31 2017 and Jan 1 2018 <div style="font-size: 70%"> https://sat-api.developmentseed.org/search/stac?datetime=2016-12-31/2017-01-01 </div>

## sat-utils
https://github.com/sat-utils
Set of utilities and command line programs for finding and fetching imagery 
- sat-api: explored above 
- https://github.com/sat-utils/sat-api 
- sat-search: client search tool 
- https://github.com/sat-utils/sat-search 
- sat-fetch: client imagery fetch tool 
- https://github.com/sat-utils/sat-fetch

# sat-search 
- Install via pip (Python2.7, Python3.6)
```
$ pip install satsearch==1.0.0rc2 $ sat-search -h $ sat-search search -h
```
- Now, let's create an Area of Interest (AOI) http://geojson.io
- Or, use one you already have
- Keep it somewhat small, size of a city

## Searching imagery Search for Landsat-8 imagery in your AOI
```
$ sat-search search --intersects aoi.geojson -c landsat-8-l1
```
Search for Landsat-8 imagery in 2016 for AOI
```
$ sat-search search --intersects aoi.geojson -c landsat-8-l1  --datetime 2016-01-01/2016-12-31 --print_cal
```
Limit to cloud cover < 20%
```
$ sat-search search --intersects aoi.geojson -c landsat-8-l1  --datetime 2016-01-01/2016-12-31 --eo:cloud_cover 0/20  --print_cal
```
Print date and scene IDs of matching scenes
```
$ sat-search search --intersects aoi.geojson -c landsat-8-l1  --datetime 2016-01-01/2016-12-31 --print_md
```

# Saving results for later Search for matching scenes and save to GeoJSON file
```
$ sat-search search --intersects aoi.geojson -c landsat-8-l1  --datetime 2016-01-01/2016-12-31 --eo:cloud_cover 0/20  --save results.geojson
```
Load results and print calendar
```
$ sat-search load results.geojson --print_cal
```
Load results and download thumbnails
```
$ sat-search load results.geojson --download thumbnail
```

# sat-fetch
- Works just like sat-search 
- Downloads imagery for just AOIs, not entire tiles 
- Requires GDAL

------

## Using sat-fetch with docker

```
$ docker run --rm -v $PWD:/home/geolambda/work -it developmentseed/geolambda /bin/bash
$ cd work $ sat-fetch load aoi.geojson --download red green blue
```

# What's next ? 
- Sat-utils is all open-source and to contribution 
- STAC spec is rapidly changing 
- What is your use case?
