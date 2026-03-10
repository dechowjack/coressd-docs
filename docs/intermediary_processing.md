# Intermediary Processing

Before Blender can run, the raw LIS model outputs and MODIS SCF data must be processed into a set of smoothed, quality-controlled input files. This step is handled by the **[`dechowjack/csd_dev`](https://github.com/dechowjack/csd_dev)** repository, which contains MATLAB scripts and supporting Bash utilities.

## Working Directory

All `csd_dev` scripts **must be run from**:

```
/discover/nobackup/projects/coressd/csd_dev
```

A symlink within this directory points to auxiliary data:

```
aux_data -> /discover/nobackup/projects/coressd/csd_aux_data/aux_data
```

## Auxiliary Data

The `aux_data` directory contains:

| File / Directory | Description |
|---|---|
| `HUC8_CA/`, `HUC8_US/` | HUC8 watershed shapefiles for Canada and the US |
| `MeanSCF/NA_MeanSCF_WYxxxx.tif` | Annual mean snow cover fraction by water year |
| `MODDEM1KM_fixed.tif` | MODIS 1 km DEM |
| `MODIS_SINUSOIDAL_fixed.prj` | MODIS sinusoidal projection definition |
| `scf_mask.mat` | Ocean / water body mask |


## Workflow

### Step 1 — Run the MATLAB pipeline

Launch MATLAB from the `csd_dev` working directory and execute the main orchestration script:

```matlab
run('csd_main.m')
```

When prompted, enter the **4-digit water year** (e.g., `2016`).

The script sequentially calls:

| Sub-script | Runtime | Output |
|---|---|---|
| `csd_gen_mean_scf_files.m` | ~10–15 min | `MeanSCF/NA_MeanSCF_WYxxxx.tif` |
| `csd_gen_psval_nc.m` | ~1 min | `PrecipScalarFiles/WYxxxx/precip_scalar.nc` |
| `csd_smooth_mass_vars.m` (snowfall) | ~5 min | `SmoothedInputs/WYxxxx/Snowf_tavg.nc` |
| `csd_smooth_mass_vars.m` (SWE) | ~15 min | `SmoothedInputs/WYxxxx/SWE_tavg.nc` |

**Total runtime:** approximately 30–40 minutes.

### Gaussian Smoother (`csd_smooth_mass_vars.m`)

The `csd_smooth_mass_vars.m` function applies a **Gaussian spatial smoother** to the snowfall and SWE fields. The smoother reduces high-frequency spatial noise in the LIS outputs before they are passed to Blender. This uses the custom `imgaussfilt_nan` function which is basically a clone of the defualt MATLAB `imgaussfilt` that has been updated to handle nan pixels and not break.

<!-- TODO: document smoother kernel parameters (sigma, window size) -->
<!-- TODO: document any edge/boundary handling at coastlines or domain borders -->
<!-- TODO: confirm whether smoothing is applied to both Snowf_tavg and SWE_tavg or only one -->

### Step 2 — Fix NetCDF metadata attributes (`fix_smoothed_attrs.sh`)

After exiting MATLAB, run the Bash metadata-fix script to correct NetCDF attributes on the smoothed output files:

```bash
bash fix_smoothed_attrs.sh 2016
```

This script iterates over the smoothed NetCDF files for the given water year and applies `ncatted` (or equivalent) to update/correct CF-convention metadata attributes (e.g., `units`, `long_name`, `_FillValue`, `grid_mapping`).

<!-- TODO: document exactly which attributes are fixed and why they need to be corrected post-MATLAB -->
<!-- TODO: note any dependency on NCO tools (ncatted, ncrename) that must be loaded as modules on Discover -->

### Step 3 — Verify output files

Confirm that the following files exist and are non-empty before proceeding to the Blender run:

```
coressd/csd_aux_data/aux_data/MeanSCF/NA_MeanSCF_WYxxxx.tif
coressd/Blender/SmoothedInputs/WYxxxx/Snowf_tavg.nc
coressd/Blender/SmoothedInputs/WYxxxx/SWE_tavg.nc
coressd/PrecipScalarFiles/WYxxxx/precip_scalar.nc
```

<!-- TODO: document how to verify file integrity (e.g., ncdump -h checks, expected variable shapes) -->

## Repository Structure

```
csd_dev/
├── csd_main.m                  # Orchestrates the full workflow
├── csd_gen_mean_scf_files.m    # Generates climatological mean SCF GeoTIFFs
├── csd_gen_psval_nc.m          # Generates precipitation scalar NetCDF
├── csd_smooth_mass_vars.m      # Gaussian smoother for Snowf_tavg and SWE_tavg
├── fix_smoothed_attrs.sh       # Bash script to fix NetCDF metadata post-MATLAB
├── aux_data -> ...             # Symlink to auxiliary data on Discover
└── functions/                  # Additional preprocessing and helper scripts
```

<!-- TODO: document functions/ subdirectory contents -->
