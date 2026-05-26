# GeoEPIC Documentation

This repository contains the public MkDocs site for GeoEPIC.

## Local Preview

```bash
python -m pip install -r requirements.txt
python -m mkdocs serve
```

Open the local URL printed by MkDocs to review changes.

## Build

```bash
python -m mkdocs build --strict
```

## Deploy

The published site is served from the `gh-pages` branch.

```bash
python -m mkdocs gh-deploy --force
```

Edit Markdown files under `docs/`. Keep command examples aligned with the shipped `geo_epic` command from the `geo-epic` package.
