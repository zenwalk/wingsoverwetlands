# Documentation of Data Sources use in the CSN Tool #

This document describes the data sources that have been used in the Critical Sites Network Tool and the steps that were taken to migrate the data into the relevant maps that are used in the CSN tool. It was written by Andrew Cottam at UNEP-WCMC.

## Overview of Data Sources used ##

There are a number of data sources that have been used in the CSN tool and these are described in the following sections. A brief description of the data is given and then any additional processing steps that were done to be able to show the data in the CSN tool. For all spatial data sources, the datasets were imported into a single ESRI File Geodatabase and projected into Web Mercator. This Geodatabase is used to create all of the maps on the CSN tool. The non-spatial data was imported into Access and for processing and then into Microsoft SQL Server 2008 for delivery into the CSN tool.

### Species information ###

Species information was obtained by joining these datasets together:

  1. Existing species list from the CSN tool version 1
  1. French names from 'IWC Species codes with BL Species RecIDs.xls' (Szabolcs Nagy 27/1/10)
  1. Species IUCN categories from WBDB (**'SpcRedList**' from Ian Fisher on 12/1/2010)

### Species Distribution Maps ###

The Species Distribution maps were provided by Wetlands International and have been published in the Waterbird Population Estimates publications. The version that is in the CSN tool dates from 2008.

### Species Population Flyways ###

The Species Population Flyways maps were provided by Birdlife International (Mark Balman on 21/1/2010).

### IBA qualifying populations and proportion of flyway population ###

The following data sources were used to derive the population estimates for sites:

  1. Waterbird Population Estimates 4 (from 'WPE4 with flyway codes\_jun09.xls' - Szabolcs Nagy on 8/1/10)
  1. IBA qualifying populations and corresponding flyways (from 'IBA\_POP.dbf' - Szabolcs Nagy on 27/1/10)
  1. IBA List (from 'IBA list.xls' - Szabolcs Nagy on 27/1/10)

Each of these datasets was imported into the Access 2003 database 'CSN\_Tool\_Data.mdb' and the following processing steps were run:

  1. 'WPE4 with flyway codes\_jun09.xls' imported as 'Data table'
  1. Changed SpcRecID data type to Number
  1. 'IBA\_POP.dbf' imported as 'IBA\_POP'
  1. 'IBA list.xls' imported as 'Site list'
  1. Added ISO CountryCode and HasPolygonData fields and populated them. Added indexes.
  1. Query 'Site Populations and Percentages1'  and 2 run to produce the CSN tool data
  1. 'Site Populations and Percentages' table imported into SQL Server 2008

### IBA List ###

The IBA List was supplied by Wetlands International (from 'IBA List.xls' - Szabolcs Nagy on 27/1/10).

  1. All Romanian sites were removed because of character set issues.
  1. CombSitRecIDText field renamed to SiteRecID
  1. IBA Polygons exported from shapefile and IBA records with polygons were flagged
  1. Data imported into an SQL Server 2008.

IBA spatial data was supplied by Wetlands International (from 'AEWA - New IBA Poly & Points.zip' - Szabolcs Nagy 20/1/10). There are some issues with this data:
  * The field are very different in the polygon and point feature classes
  * There are some features in the polygon feature class that have no names!
  * Extended characters have not been captured properly - there are site names with weird characters in, e.g. 22155 is '“Paradise Valley” mountain plateau'
  * Many of the IBAs have an area of 0
  * There are IBAs in the IBA List that are not present in the spatial data, e.g. Gibraltar Point (http://dev.unep-wcmc.org/WingsOverWetlands/default.html#app=55ec&51b9-currentState=Sites&51b9-stateID=20810)

Do we use national or international name. For example, SiteRecID 3234 has the name 'Untere Traun' in the spatial data and 'Lower course of the Traun' in the WBDB.

### IWC List ###

The IWC site list was supplied by Wetlands International (from 'Consolidated IWC site codes with matching IBA Ids.xls' - Szabolcs Nagy 27/1/10)

  1. Query run to create table in SQL Server (_IWC List)
  1. Data imported into SQL Server
  1. Spatial data created in ArcGIS from Lat/Long values and reprojected to Web Mercator (IWC\_List\_Species feature class)_

### IWC Counts ###

The IWC counts were supplied by Wetlands International (from 'IWC COUNT.dbf). The data were reformatted to average counts for each year/species/site combination.


# Preparation of data for the filter tables #

## Core tables ##
The filter tables use a spine of F\_SPECIES, F\_SPECIES\_FLYWAY, F\_SPECIES\_FLYWAY\_LINK AND F\_IBA. These are all created from the CSN_**tables with queries in the e:\gisdata\WingsOverWetlands folder, named Filter**.sql._

The lookup tables and link tables that link to the core spine tables were created in the Access database SpeciesWBDBdataForAndy.mdb using the queries F