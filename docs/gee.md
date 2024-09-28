Google Earth Engine (GEE) is a cloud-based platform for planetary-scale environmental data analysis. It hosts a vast collection of satellite imagery and other geospatial datasets, enabling users to perform large-scale data analysis.

This module can be used to extract remote sensing satellite data directly from Google Earth Engine.

<img src="../assets/gee.png" alt="soil" width="80%"/>

### 1. Configuration file breakdown
This module uses a configuration file in yaml format to extract data from google earth engine. This configuration file defines the global parameters, satellite collections, and derived variables . Below is a detailed explanation of each section.

```yaml 
# filename: config.yml

# Global parameters
global_scope:
  time_range: ['2016-01-01', '2021-12-31']
  variables: ['nir', 'red', 'green', 'ndvi', 'lai_ndvi']  
  resolution: 10

# Specify Earth Engine collections and their respective variables
collections:

  le07:
    collection: LANDSAT/LE07/C02/T1_L2
    select:  (b('QA_PIXEL') >> 6) & 1
    variables:
      nir: b('SR_B4')*0.0000275 - 0.2
      red: b('SR_B3')*0.0000275 - 0.2
      green: b('SR_B2')*0.0000275 - 0.2

# Derived variables
derived_variables:
  ndvi: '(nir - red)/(nir + red)'

```
#### i. Global Parameters

The `global_scope` section contains general settings applicable to the entire data extraction process.

- **`time_range`**: Specifies the period for which the satellite data should be fetched.

- **`variables`**: Lists the key variables to be extracted. These are typically satellite bands or derived products such as vegetation indices.

- **`resolution`**: Defines the spatial resolution (in meters) for the output data.

#### ii. Satellite-Specific Parameters

Each satellite dataset is defined under its own subsection. The key components are `collection`, `select`, and `variables`. Multiple satellite dataset can be given. If data from multiple satellites are present for the same time period, the module will return the mean of the variables across all satellites.

- **`collection`**: Refers to the specific Google Earth Engine dataset for LANDSAT 7.

- **`select`**: Defines a masking condition for the data selection.

- **`variables`**: Specifies the bands to extract, along with any scaling factors.

#### iii. Derived Variables

Derived variables are calculated from the raw bands using mathematical expressions and functions available in numpy package.

---

### 2. Fetching the Data

You can use the following command line to fetch satellite data from Google Earth Engine and store it in a CSV file:

```bash
GeoEPIC gee <config-file> --fetch <input> --out <output-path>
```

#### i. Input options

The input for fetching data can be provided in three formats:

   - **Latitude and Longitude:** Use direct coordinates (latitude and longitude)
```bash
GeoEPIC gee ./landsat_lai.yml --fetch 40.56773135493621 98.55058401654036 --out ./out/sample.csv
```
   - **Shapefile (.shp):** A shapefile containing the region of interest. It must contain a SiteID or FieldID column. The data will be stored in the name of FieldID or SiteID.
```bash
GeoEPIC gee ./landsat_lai.yml --fetch ./input/region.shp --out ./out
```

   - **CSV File:** A CSV file must contain a SiteID or FieldID column. Additionally, if using a CSV, it must include lat, lon columns. The data will be stored in the name of FieldID or SiteID.
```bash
GeoEPIC gee ./landsat_lai.yml --fetch ./input/region.csv --out ./out
```