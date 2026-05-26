# Installation

GeoEPIC should be installed in a Conda environment because GDAL and related geospatial libraries are compiled dependencies. The current recommended Python version is 3.11.

## Option 1: Local clone setup

Use this path when you have cloned the `geo-epic` repository and want the setup script to create or reuse the Conda environment. The script installs GeoEPIC from the local checkout with `python -m pip install .`.

```bash
git clone https://github.com/smarsGroup/geo-epic.git
cd geo-epic
bash installation_scripts/setup.sh
conda activate epic_env
```

If an older `epic_env` already exists with a different Python version, remove it first:

```bash
conda env remove -n epic_env
bash installation_scripts/setup.sh
conda activate epic_env
```

## Option 2: Pip install

Use this path when you want to manage the Conda environment yourself and install GeoEPIC directly from GitHub.

```bash
conda create -n epic_env python=3.11 pip -y
conda activate epic_env
conda install -c conda-forge gdal=3.7 pygmo=2.19.5 -y
python -m pip install git+https://github.com/smarsGroup/geo-epic.git
geo_epic init
```

## Verify the installation

```bash
geo_epic
python -c "import geoEpic, pygmo; print(geoEpic.__file__, pygmo.__version__)"
```

The command-line entry point is `geo_epic`. Run it from the activated Conda environment.
