# Weather Module

<!-- ![weather](./assets/fg.jpg) -->
<img src="../assets/fg.jpg" alt="weather" width="90%"/>

Weather data is vital in providing essential environmental inputs that significantly influence crop growth and development. Reliable weather inputs ensure that agricultural simulations reflect realistic responses to climatic conditions. The EPIC model requires weather input files that detail daily and monthly climatic variables. Daily files provide day-to-day weather data, while monthly files summarize the average or total values per month. These files are crucial for driving the daily simulation processes in EPIC.

## Weather preparation commands

The current weather CLI is focused on preparing Daymet, NLDAS windspeed, daily grids, and monthly weather summaries.

```bash
# Download Daymet weather for an AOI shapefile
geo_epic weather daymet -start 2002-01-01 -end 2022-12-31 -aoi ./fields.shp -wd ./weather

# Prepare NLDAS windspeed data using config.yml
geo_epic weather windspeed -c config.yml

# Download daily weather grids using config.yml
geo_epic weather download_daily -c config.yml

# Convert daily weather files to monthly summaries
geo_epic weather daily2monthly -i ./weather/Daily -o ./weather/Monthly
```

`windspeed` and `download_daily` read dates, output folders, and region settings from the workspace configuration. Inspect the command options with:

```bash
geo_epic weather daymet -h
geo_epic weather windspeed -h
geo_epic weather download_daily -h
geo_epic weather daily2monthly -h
```

## Earth Engine time-series extraction

For arbitrary Earth Engine collections and derived variables, use the GEE utility command instead of the weather module:

```bash
geo_epic utility gee daily_weather.yml --fetch ./fields.shp --out ./weather/gee_timeseries
```

GeoEPIC allows the integration of various weather and climate data sources on GEE. To explore available datasets, visit [Google Earth Engine's dataset catalog](https://developers.google.com/earth-engine/datasets/) and [GEE Community Catalog](https://gee-community-catalog.org/projects/agera5_datasets/). Private assets can also be uploaded to Earth Engine.

**Example AgERA5 configuration:**

- **using AgERA5**

```yaml
# Global parameters
global_scope:
  time_range: ['2002-01-01', '2022-12-31']
  variables: ['srad', 'tmax', 'tmin', 'prcp', 'rh', 'ws']  
  resolution: 9600


# Specify Earth Engine (EE) collections and their respective variables
collections:
  AgEra5:
    collection: 'projects/climate-engine-pro/assets/ce-ag-era5/daily'
    variables:
      srad: b('Solar_Radiation_Flux') 
      tmax: b('Temperature_Air_2m_Max_24h') - 273.15
      tmin: b('Temperature_Air_2m_Min_24h') - 273.15
      prcp: b('Precipitation_Flux') 
      rh: b('Relative_Humidity_2m_06h')
      ws: b('Wind_Speed_10m_Mean')
```

<!-- 

### Sources

- **DAYMET** : [https://daymet.ornl.gov/](https://daymet.ornl.gov/) 
- **NLDAS** : [https://climatedataguide.ucar.edu/climate-data/nldas-north-american-land-data-assimilation-system/](https://climatedataguide.ucar.edu/climate-data/nldas-north-american-land-data-assimilation-system/)  -->
