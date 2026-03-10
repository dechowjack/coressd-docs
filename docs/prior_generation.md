# Prior Generation

The Blender algorithm requires a land-surface model prior for SWE and snowfall. Two input data streams feed the prior: (1) model state variables from a NoahMP open-loop run, and (2) MODIS satellite snow cover fraction.

## Land Surface Model: NoahMP 4.0.1 via NASA LISF

The model prior is generated using **NoahMP version 4.0.1** within the **NASA Land Information System Framework (LISF)**. The open-loop (no data assimilation) run is forced with **MERRA-2** reanalysis meteorology at 1 km spatial resolution over North America.

Outputs are produced by collaborator M. Wrzesien and stored at:

```
/discover/nobackup/projects/coressd/mwrzesie/runOL/out.OL/SURFACEMODEL/
```

Variables extracted for use as Blender inputs:

| Variable | NetCDF name | Notes |
|---|---|---|
| Snow Water Equivalent | `SWE_tavg` | Daily mean |
| Snowfall flux | `Snowf_tavg` | Daily mean |
| Near-surface air temperature | `Tair_f_tavg` | Daily mean |
| Ground heat flux | `Qg_tavg` | Daily mean; also used as spatial template/mask |

<!-- TODO: confirm NoahMP version, LISF version, MERRA-2 product version (M2SDNXSLV or similar), and run period -->
<!-- TODO: add spatial extent and projection details for the LIS grid -->
<!-- TODO: note any spin-up period or water year coverage -->

## Snow Cover Fraction: MOD10A1F CGF

Satellite SCF observations are derived from the **MODIS/Terra Snow Cover Daily L3 Global 500m Grid** gap-filled (CGF) product (**MOD10A1F**), which fills cloud-obscured pixels using prior clear-sky observations.

### Tree Cover Correction

Raw MOD10A1F SCF is corrected for forest canopy interception bias using **MOD44B** (MODIS Vegetation Continuous Fields, 500 m annual tree cover). The correction adjusts SCF in forested pixels to account for canopy masking of the underlying snowpack.

The tree-cover processing notebook is located at:

```
/discover/nobackup/projects/coressd/Github/Blender_notebooks/Modis/Forest/05_MOD44B_on_Discover.ipynb
```

> This notebook should be run directly from JupyterHub on Discover. The tree-cover data have already been processed; re-running is only necessary if updated MOD44B tiles are downloaded.

### Resampling

After tree-cover correction, MOD10A1F SCF is clipped and resampled from **500 m to 1 km** to match the LIS/NoahMP grid, using `Qg_tavg.nc` as the spatial template and land mask.

Processed script: `Python/process_modis_cgf.py` in the `verse` repo.

Intermediate outputs:

```
/discover/nobackup/projects/coressd/Blender/Modis/MOD10A1F/clipped_WYxxxx/
```

Final SCF input for Blender:

```
/discover/nobackup/projects/coressd/Blender/Inputs/WYxxxx/SCF.nc
```

<!-- TODO: document SCF uncertainty parameterization used in Blender -->
<!-- TODO: document any additional quality filtering applied to MOD10A1F -->
