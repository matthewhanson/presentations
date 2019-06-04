<div>STAC related updates</div>
<hr />
<div>
    <img src='assets/e84-logo.png' style="border:none; height:30%; width:30%; box-shadow:none;" />
    <div style="text-align: right; font-size: 80%">
        Matthew Hanson<br />
        Element 84<br />
        @geoskeptic
    </div>
</div>

---

## What is STAC?

#### 1. Metadata Model
set of standard fields for geospatial data

#### 2. catalog (static)
linked series of files that can be crawled

#### 3. api (dynamic catalog)
RESTful API for querying geospatial data (uses WFS3)

---

## 1. STAC Metadata

#### Catalogs
contains links to other Catalogs, Collections and Items

#### Collections
A set of Items that share common metadata

#### Items
A single record/scene for datetime and location

----

### Pangeo

- intake-stac
    - https://github.com/pangeo-data/intake-stac
- STAC CMR
    - https://cmr-stac-api.dev.element84.com/
    - https://github.com/Element84/cmr-stac-api-proxy





### sat-utils

https://github.com/sat-utils

- sat-api
- sat-stac
- sat-search


## Open source STAC software

- STAC validator (James Banting, SparkGeo)
- Staccato (Josh Fix, Boundless/Planet)
- CMR STAC (Jason Gilman, Element 84)
- sat-utils
    - family of tools
    - https://github.com/sat-utils

----

## sat-stac

- Create your own STAC catalogs
- Used for creating Landsat and Sentinel catalogs at
  - https://landsat.stac.cloud
  - https://sentinel.stac.cloud
  - http://cbers.stac.cloud/
- Python library for working with STAC entities
- Jupyter Tutorials

----

### sat-api
- h/t Development Seed, Alireza, Sean Harkins
- STAC dynamic API reference implementation
- Node library for a (STAC) API: https://github.com/sat-utils/sat-api
- Deployment project for deploying your own API: https://github.com/sat-utils/sat-api-deployment
- Live API of Landsat-8 and Sentinel-2 datasets


## sat-search

- Python CLI and library for searching STAC compliant endpoints

- Install via pip (Python3 only)

```
$ pip install satsearch
$ sat-search -h
$ sat-search search -h
```

----

### sat-fetch
- Works just like sat-search 
- Downloads imagery for just AOIs, not entire tiles 
- Requires GDAL

---

### Earth Search

Earth Search: https://www.element84.com/earth-search/

- Landsat-8
- Sentinel-2
- CBERS-4

Coming
- Sentinel-1
- NAIP
- MODIS

---
