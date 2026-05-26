# Running the Model

GeoEPIC exposes the modeling classes from `geoEpic.core` and output readers from `geoEpic.io`.

```python
from geoEpic.core import Site, EPICModel
from geoEpic.io import ACY, DGN

# Initialize a site object.
site = Site(
    opc="./continuous_corn.OPC",
    dly="./1123455.DLY",
    sol="./Andisol.SOL",
    sit="./1.SIT",
)


# Initialize the EPIC model.
model = EPICModel(path="./model/EPIC2301dt20230820")
model.setup({
    "start_year": 2014,
    "duration": 10,
    "output_dir": "./output",
    "log_dir": "./log",
})
model.set_output_types(["ACY", "DGN"])

# Run the simulation for the site.
model.run(site)

# Read output files.
acy = ACY(site.outputs["ACY"])
dgn = DGN(site.outputs["DGN"])
model.close()
```

## Loading from config files

The configuration workflow is best handled by `Workspace`, which reads `config.yml`, filters `run_info`, runs EPIC, and applies optional post-processing routines.

```python
from geoEpic.core import Workspace

workspace = Workspace("./config.yml")
workspace.run()
```

Example model section:

```yaml
EPICModel: ./model/EPIC2301dt20230820
start_year: 1995
duration: 25
output_types:
  - ACY  # Annual Crop data file
  - DGN  # Daily general output file
log_dir: ./log
output_dir: ./output
```

To copy packaged utilities such as the EPIC editor into the current workspace, use:

```bash
geo_epic workspace copy epic_editor
```

Known limitation: import modeling classes from `geoEpic.core`. The top-level `geoEpic` package currently exposes dispatcher helpers rather than `Site`, `EPICModel`, or `Workspace`.
