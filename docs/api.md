# Command and Python API Reference

GeoEPIC installs the `geo_epic` command. The general command shape is:

```bash
geo_epic [module] [function] [options]
```

Use `-h` on any command to inspect the current options:

```bash
geo_epic workspace new -h
geo_epic soil usda -h
geo_epic utility gee -h
```

## Command modules

### Initialization

| Command | Purpose |
| --- | --- |
| `geo_epic init` | Initialize GeoEPIC metadata in the user home directory. |

### Workspace

| Command | Purpose |
| --- | --- |
| `geo_epic workspace new -n <workspace>` | Create a workspace from the packaged template. |
| `geo_epic workspace prepare -c config.yml` | Prepare workspace inputs from a configuration file. |
| `geo_epic workspace validate -c config.yml` | Validate site, weather, soil, and management inputs. |
| `geo_epic workspace run -c config.yml` | Run EPIC simulations for the configured sites. |
| `geo_epic workspace post_process -c config.yml` | Run configured post-processing. |
| `geo_epic workspace visualize -c config.yml` | Run configured visualization. |
| `geo_epic workspace copy <source> [destination]` | Copy packaged utilities such as `epic_editor` into a workspace. |

### Soil

| Command | Purpose |
| --- | --- |
| `geo_epic soil usda --fetch <lat> <lon> --out <dir>` | Fetch USDA SSURGO soil data and write `.SOL` files. |
| `geo_epic soil usda --fetch <locations.csv> --out <dir>` | Fetch SSURGO data for a CSV with location columns. |
| `geo_epic soil usda --fetch <fields.shp> --out <dir>` | Fetch SSURGO data for shapefile centroids. |
| `geo_epic soil process_gdb -c config.yml` | Process a local gSSURGO geodatabase using workspace config paths. |

### Weather

| Command | Purpose |
| --- | --- |
| `geo_epic weather daymet -start <date> -end <date> -aoi <fields.shp> -wd <dir>` | Download Daymet weather for an AOI. |
| `geo_epic weather windspeed -c config.yml` | Prepare NLDAS windspeed data from config. |
| `geo_epic weather download_daily -c config.yml` | Download daily weather grids from config. |
| `geo_epic weather daily2monthly -i <input> -o <output>` | Convert daily weather files to monthly summaries. |

### Utility

| Command | Purpose |
| --- | --- |
| `geo_epic utility gee <config.yml> --fetch <lat> <lon> --out <file.csv>` | Extract Earth Engine time series for one point. |
| `geo_epic utility gee <config.yml> --fetch <fields.shp> --out <dir>` | Extract Earth Engine time series for many fields. |
| `geo_epic utility crop_csb <input.gdb> <output.shp>` | Clip and process Crop Sequence Boundaries. |
| `geo_epic utility change_ee_project -n <project>` | Change the configured Earth Engine project. |
| `geo_epic utility generate_opc -c crop_data.csv -t crop_templates -o files` | Generate OPC files from crop data and templates. |

### Sites

| Command | Purpose |
| --- | --- |
| `geo_epic sites generate -c config.yml` | Generate EPIC `.SIT` files from configured run information. |

## Python imports

Use the package submodules directly:

```python
from geoEpic.core import Site, EPICModel, Workspace, PygmoProblem
from geoEpic.io import ACY, DGN, DLY, OPC, SOL, SIT, CropCom, ieParm
from geoEpic.spatial import Daymet, DEM, SSURGO, SoilGrids
```

The top-level `geoEpic` package currently exposes the dispatcher, not the modeling classes. Import modeling classes from `geoEpic.core`.

## Important notes

- Run the installed `geo_epic` command from an activated environment. Running source scripts directly can fail because helper scripts rely on package imports.
- The workspace template and `workspace prepare` currently disagree on `Area_of_Interest` versus `Fields_of_Interest`. For `workspace prepare`, the code expects `Fields_of_Interest`.
