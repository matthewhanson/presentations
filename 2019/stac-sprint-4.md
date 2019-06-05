<div>STAC software efforts</div>
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

## sat-utils

https://github.com/sat-utils

- Collection of STAC related software efforts
    - sat-api
    - sat-stac
    - sat-search
    - sat-fetch
    - sat-stac-landsat
    - sat-stac-sentinel

----

## sat-api

https://github.com/sat-utils/sat-api

- STAC dynamic API reference implementation
- Node library for a (STAC) API: https://github.com/sat-utils/sat-api
- Deployment project for deploying your own API: https://github.com/sat-utils/sat-api-deployment

----

## sat-stac

https://github.com/sat-utils/sat-stac

- Create your own STAC catalogs
- Used for creating Landsat and Sentinel catalogs at
  - https://landsat.stac.cloud
  - https://sentinel.stac.cloud
  - http://cbers.stac.cloud/
- Python library for working with STAC entities
- Jupyter Tutorials

----

## sat-stac-landsat

https://github.com/sat-utils/sat-stac-landsat

- Index Landsat scenes on AWS
- Transform Landsat metadata to STAC
- Lambda function for publishing Landsat STAC Items

----

## sat-stac-sentinel

https://github.com/sat-utils/sat-stac-sentinel

- Sentinel-2, Sentinel-1 coming
- Index Sentinel scenes on AWS
- Transform Sentinel metadata to STAC
- Lambda function for publishing Sentinel STAC Items

----

## sat-search

https://github.com/sat-utils/sat-search

- Python CLI/library for searching STAC compliant endpoints

```
$ pip install satsearch
$ sat-search -h
$ sat-search search -h
```

<img src='assets/sentinel-ngorongoro-cal.png' style="border:none; height:35%; width:35%;" />

----

### sat-fetch

https://github.com/sat-utils/sat-fetch

- Works just like sat-search 
- Downloads imagery for just AOIs, not entire tiles 
- Requires GDAL

----

## sat-gbdx

- like sat-search, but for DG
- Unreleased
- Client proxy library/CLI to Digital Globe GBDX

----

## intake-stac

https://github.com/pangeo-data/intake-stac

- Plugin to Intake
- Dynamically process STAC assets with dask
- Part of Pangeo project

----

## STAC CMR

https://cmr-stac-api.dev.element84.com/
https://github.com/Element84/cmr-stac-api-proxy

- STAC 0.5.0 right now
- Proxy to CMR
    - STAC query in
    - STAC Items out

----

## sat-api browser

https://github.com/developmentseed/sat-api-browser

- Front-end to a STAC API
- Recently opened by Development Seed

---

## Earth Search

https://www.element84.com/earth-search/

API: https://earth-search.aws.element84.com/stac

- Current datasets
    - Landsat-8
    - Sentinel-2
    - CBERS-4
- Coming
    - Sentinel-1
    - NAIP
    - MODIS
