---
layout: default
title: cosmic pages
---

# Data Sharing in Cosmic Sense

## Preface

Within CosmicSense, efficient data exchange a) facilitates cooperation, b) enables later publication in an ISI-listed data journal, and c) addresses the data management plan promised in the proposal.

This document is an attempt to suggest conventions for a file-based data exchange. It seeks a compromise between reasonable effort for the data provider and re-usability for the data user.

Feel free to discuss the details, e.g. by adding comments (`Ctrl+Alt+M`).

## Access the share space

We use the EUDAT / European Open Science Cloud (EOSC) [B2DROP](https://b2drop.eudat.eu) service for data sharing. There, we created a directory `cosmicsense` which will hold the shared data. You can access the directory in one of the following ways:

- Use the **web interface** via this link share: https://b2drop.eudat.eu/s/YsfPDGkrNCRNH5W (password has been send to you)

- **Log into the EUDAT system** (*preferred*). Send us your user name, so we can share the directory with you. That way, you will be able to add the directory to your local file system via [WebDAV](https://eudat.eu/services/userdoc/b2drop#UserDocumentation-B2DROPUsage-WebDavclient). **How to log in?** You can either use your institutional account (preferred, but does not always work) or you can register a new account at https://b2access.eudat.eu.

## Directory structure and concept

The data exchange is based on a shared file system (“cloud”). In this cloud, two data directories exist - `inbox` and `homogenized`. 

`inbox` is the prime destination for any data upload. If the data comply to the conventions, or once they have been adjusted accordingly, they will be moved to `homogenized`. *Please do not upload any data directly to homogenized unless you are certain that your data comply!*

Within both folders, the data are organised hierarchically:

```
[Site]
   [geodata/time series/others]
      [variable / sensor]
```

The additional folder `docs` contains further technical documents; the folder `misc` is intended for exchanging other files such as presentations, literature, etc.

## Conventions

For ease of use, we recommend the following standards for exchanging scientific data:

### File names
If applicable: `[stationid]_[variable].[extension]`

### File formats

| **Data type** | **Format (extension)** | **Details** |
| :------------ | :--------------------- | :---------- |
| time series, tables | csv-like (.txt) | ... |
| static grids   | GeoTIFF | ... |
| space-time grids | to be specified (e.g. NetCDF acc. to CF-Conventions) | ... |
| spatial vector data | Shapefile (.shp) or geojson | ... |

<br>

### Spatial reference

WGS-84 (EPSG code: 4326)

### Temporal reference
Specify date / time according to [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) in UTC (example: `2019-01-23` for date, `2019-01-23T06:42:14Z` for datetime)

For any time series, metadata have to specify whether the data represents instantaneous or time-integrated/averaged values, and, for the latter, the length of the interval, and whether the time stamp represents the start or the end of that interval.

### Variables and units

| **Variable** | **Unit** | **Remarks** |
| :----------- | :------- | :---------- |
| temperature | degC | ... |
| soil moisture | [m3/m3] | volumetric water content |
| lat, lon | decimal degrees | ... |
| barometric pressure | mbar | ... |
| relative humidity | [-] | ... |
| precipitation | mm | timestamp denotes end of interval |
| streamflow | m³/s | ... |
| ground water level | m a.s.l. | ... |
| neutron counts | counts / interval | timestamp denotes end of interval |

<br>

## Meta data
If you are wondering which metadata to provide, let only one question guide you: **Will others, on the basis of that metadata, be able to use my data?**

The set of metadata we suggest in the following is far from exhaustive and will certainly not cover every use case. Feel free to enhance that minimum set by any documentation you consider helpful.

For every file (or folder, if applicable), a minimum information on meta data needs to be specified. The selection has loosely been inspired by the DCMI (http://www.dublincore.org/documents/dces/), seeking a compromise between completeness and effort.

Please use the template `docs/meta_data_template.json`. Please provide all required information (if applicable), unless specified otherwise (e.g by projection files for GIS data):

```
# meta-data documentation file
# place this template into the same folder as the respective file and rename it to [file_name]_meta.txt
# for multiple similar files in (multiple) folders, this file may also be placed in the same (parent) folder.
{
   "Provider": {
      "Subject": "Contact of the person that uploaded the data."
      "Name": "",
      "Institution": "",
      "Email": ""
   },
   "Coverage": {
      "Subject": "Spatial and temporal coverage of the overall dataset; RegionName can e.g. be a site name such as 'Schaefertal' or 'Fendt'; BBox is a list of [left, right, bottom, top]"
      "StartTime": "YYYY-MM-DD",
      "EndTime": "YYYY-MM-DD",
      "RegionName": "",
      "BBox": [] 
   },
   "Source": {
      "Subject": "Source of the original data set."
      "Name": "",
      "Institution": "",
      "LinkToOriginalSource": ""
   },
   "Description": {
      "Subject": "Brief, but meaningful description of the data set, e.g. 'Precipitation time series collected in Wüstebach catchment'",
      "Description": ""
   },
   "Units": {
      "Subject": "List of units used in the actual data, if any.",
      "Units": ["", "", ...]
   },
   "Units": {
      "Subject": "List of units used in the actual data, if any.",
      "Units": ["", "", ...]
   },
   "SpatialReference": {
      "Subject": "Spatial reference system, in case of geospatial data, as short name and EPSG code as integer",
      "Name": "",
      "EPSG": integer 
   }
   "TemporalReference": {
      "Subject": "Temporal reference system, in case of time series or intermittent data. Specify time zone as 'UTC+x'; IntervalLength in seconds; in case of instantaneous observations set IntervalLength to 0.",
      "TimeZone": "",
      "IntervalLength": integer,
      "TimeIsEndOfInterval": True or False
   }
}
```
