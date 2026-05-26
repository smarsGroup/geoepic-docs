# Crop Management

OPC files describe EPIC operation and crop-management schedules. They include planting, harvest, tillage, fertilizer, irrigation, and other management operations used during a simulation.

GeoEPIC can copy existing OPC files into a workspace, edit them through the Python `OPC` class, or generate a year-wise OPC sequence from crop templates.

## Generate OPC files

Use `geo_epic utility generate_opc` when you have a crop sequence CSV and a folder of crop OPC templates.

```bash
geo_epic utility generate_opc -c ./crop_data.csv -t ./crop_templates -o ./opc/files
```

The crop data CSV must include:

- `crop_code` and `year`
- optional `planting_date` and `harvest_date` in `YYYY-MM-DD` format

The template folder must include:

- `MAPPING`, a CSV mapping `crop_code` to template names
- `FALLOW.OPC`, used when a crop code is missing from the mapping
- one OPC template file for each mapped crop name

`cdl_code` or `epic_code` columns are accepted and renamed internally to `crop_code`.

## Edit OPC files in Python

```python
from datetime import datetime

from geoEpic.io import OPC

opc = OPC.load("./opc/continuous_corn.OPC", start_year=2020)
opc.edit_fertilizer_rate(rate=120, year=2021)
opc.edit_crop_season(
    new_planting_date=datetime(2021, 4, 25),
    new_harvest_date=datetime(2021, 10, 10),
    crop_code=1,
)
opc.save("./opc/continuous_corn.OPC")
```

Refer to the EPIC user manual for complete OPC operation-code definitions and formatting details.
