---
layout: default
title: cosmic pages
---

# Data Sharing in Cosmic Sense

## Preface

Within CosmicSense, efficient data exchange a) facilitates cooperation, b) enables later publication in an ISI-listed data journal, and c) addresses the data management plan promised in the proposal.

This document is an attempt to suggest conventions for a file-based data exchange. It seeks a compromise between reasonable effort for the data provider and re-usability for the data user.

Feel free to discuss the details, e.g. by [raising an issue](https://github.com/cosmic-sense/misc/issues).

## Access the shared space

We use the EUDAT / European Open Science Cloud (EOSC) [B2DROP](https://b2drop.eudat.eu) service for data sharing. There, we created a directory `cosmicsense` which will hold the shared data. You can access the directory in one of the following ways:

- Use the **web interface** via this [link](https://b2drop.eudat.eu/s/YsfPDGkrNCRNH5W) (password has been send to you)

- **Log into the EUDAT system** (*preferred*). Send us your user name, so we can share the directory with you. That way, you will be able to add the directory to your local file system via [WebDAV](https://eudat.eu/services/userdoc/b2drop#UserDocumentation-B2DROPUsage-WebDavclient). **How to log in?** You can either use your institutional / Github / ORCID account (preferred, but does not always work) or you can register a new account at https://b2access.eudat.eu.

## Directory structure and concept

The data exchange is based on a shared file system (“cloud”). In this cloud, two data directories exist - `inbox` and `homogenised`. 

`inbox` is the prime destination for any data upload. If the data comply to the conventions, or once they have been adjusted accordingly, they will be moved to `homogenised`. *Please do not upload any data directly to* `homogenised` *unless you are certain that your data comply!*

Within both folders, the data are organised hierarchically:

```
[Site]
   [geodata/time series/others]
      [variable / sensor]
```

The additional folder `docs` contains further technical documents; the folder `misc` is intended for exchanging other files such as presentations, literature, etc.

## Conventions

As pointed out above, you can upload to `inbox` without the need to follow conventions. In that case, still remember to specify the [minimal set of metadata](#Meta-data).

For the `homogenised` directory, though, we aim for the following conventions (*subject to discussion!!*):

### File names
If applicable: `[stationid]_[variable].[extension]`

### File formats

| **Data type** | **Format (extension)** | **Details** |
| :------------ | :--------------------- | :---------- |
| time series, tables | csv-like (.txt) | tab-delimited, "." as decimal sep, "NA" for no data |
| static grids   | GeoTIFF | ... |
| space-time grids | to be specified (e.g. NetCDF acc. to CF-Conventions) | ... |
| spatial vector data | Shapefile (.shp) or geojson | ... |

<br>

### Spatial reference

WGS-84 (EPSG code: 4326)

### Temporal reference
Specify date / time according to [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) in UTC (i.e. UTC+1 (standard winter time) minus 1 hour; example: `2019-01-23` for date, `2019-01-23T06:42:14Z` for datetime)

For any time series, [metadata](#Meta-data) have to specify whether the data represents instantaneous or time-integrated/averaged values, and, for the latter, the length of the interval, and whether the time stamp represents the start or the end of that interval.

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
<a name="Meta-data"></a>
## Meta data
If you are wondering which metadata to provide, let only one question guide you: **Will others, on the basis of that metadata, be able to use my data?**

The set of metadata we suggest in the following is far from exhaustive and will certainly not cover every use case. Feel free to enhance that minimum set by any documentation you consider helpful.

For every file (or folder, if applicable), a minimum information on meta data needs to be specified. The selection has loosely been inspired by the [DCMI](http://www.dublincore.org/documents/dces/), seeking a compromise between completeness and effort.

Please use the template `docs/meta_data_template.json`. Place the template into the same folder as the respective file and rename it to `[file_name]_meta.json`. For multiple similar files in (possibly multiple) subdirectories, this file may also be placed in the parent folder, only. Please specify the information (if applicable), unless they are unambigiously specified otherwise (e.g by projection files for GIS data).

```
{
   "Description": "Brief, but meaningful description, e.g. 'Rain gauge observations at Fendt site'",
   
   "Provider": {
      "Name": "The name of the person who uploaded the data",
      "Institution": "The name of his/her institution",
      "Email": "His/her E-Mail address",
      "Comment": "Optional, for further comments..." 
   },
   
   "SpaceTimeCoverage": {
      "StartDate": "For timeseries: the start of the series in YYYY-MM-DD",
      "EndDate": "For timeseries: the end of the series in YYYY-MM-DD",
      "RegionName": "An informative name of the region: e.g. Schaefertal, Fendt, Germany, ...",
      "BBox": "Optional: meaningful specification of spatial bounding box in lat/lon" 
   },
   
   "Source": {
      "Name": "Optional: the name of the person who created the data",
      "Institution": "Optional: the institution who created the data",
      "LinkToOriginalSource": "Optional: link to the original data set or its documentation"
   },
   
   "Units": "If applicable: specification of physical units of the data",
   
   "SpatialReferenceSystem": {
      "Name": "For geospatial data, a brief common name of the spatial reference system (SRS), e.g. WGS 84",
      "EPSG": "The EPSG code of the SRS, e.g. 4326 for WGS 84" 
   },
   
   "TemporalReferenceSystem": {
      "TimeZone": "For time series, standard time zone notation, e.g. UTC or UTC+x",
      "IntervalLength": "For time series, the length of the accumulation/averaging interval (in seconds); use 0 for instantaneous data such as manual FDR observations",
      "TimestampAtEndOfInterval": "For time series, "True": timestamps refer to the end of the accumulation/averaging interval; "False": timestamps refer to the beginning of the accumulation/averaging interval."
   },
   
   "Remarks": "Optional: add any further remarks or comments you consider important (e.g. references to further accompanying explanatory files)."
}
```

<a name="qc"></a>
## Quality control and commenting

The data uploaded to `inbox` can be evaluated / commented on / tagged by using [b2note](https://eudat.eu/catalogue/B2NOTE) functionality: 
In the [web interface](https://b2drop.eudat.eu), click on the "shared" icon right of a file or folder. In the panel on the right, you can now assign tags or add comments.

To facilitate searching, automatic retrieval of comments and generation of overviews, a set of tags should be used. These are listed in `docs/qc_tags.txt`:
```
"missing"	followed by further explanation in the comments, lists missing part of data or meta-data thereof
"corrupted"	add file name(s) that cannot be read to comments
"unclear"	add issue that needs to be clarified to comments
"ok"	indicating that the data are formally ready to be transferred to "homogenized". Tags indicating problems that have been solved by then should be removed.
"homogenized"	indicating that (a homogenized version of) the data have been copied to "homogenized".
```