# Running Blender

This page describes how to execute the Blender Julia code on NASA Discover for a full water-year run. All commands are run from the `Github` directory:

```
/discover/nobackup/projects/coressd/Github
```

The algorithm code lives in [`bidhya/verse`](https://github.com/bidhya/verse). See [Algorithm](algorithm.md) for an overview of what Blender does.

## Prerequisites

### Required Julia Packages

The following Julia packages must be installed in the project Julia environment:

```
JuMP  Ipopt  Rasters  NCDatasets  CSV  LoggingExtras  Distributions
```


### Required Input Files

Before running Blender, ensure all input files for the target water year are present (see [Intermediary Processing](intermediary_processing.md)):

```
/discover/nobackup/projects/coressd/
├── Blender/
│   ├── Inputs/WYxxxx/
│   │   ├── Tair_f_tavg.nc   # air temperature (from LIS)
│   │   ├── Qg_tavg.nc       # ground heat flux (from LIS)
│   │   └── SCF.nc           # MODIS CGF SCF, tree-corrected and resampled
│   └── SmoothedInputs/WYxxxx/
│       ├── Snowf_tavg.nc    # smoothed snowfall (from csd_dev)
│       └── SWE_tavg.nc      # smoothed SWE (from csd_dev)
└── PrecipScalarFiles/WYxxxx/
    └── precip_scalar.nc     # precip scalar file, created in csd_dev
```

## Phase 1 — Generate Input Files

Two sequential SLURM jobs prepare input data (run from `Slurm_Blender/`):

### Job A: Process LIS outputs

```bash
sbatch Slurm_Jobs/a_lis_process.sh
```

Calls `Python/Extract_LIS.py`. Processes daily LIS NetCDF files and stages them into `Blender/Inputs/WYxxxx/`.

### Job B: Process MODIS CGF snow cover

```bash
sbatch Slurm_Jobs/b_process_modis_cgf.sh
```

Calls `Python/process_modis_cgf.py`. **Hard dependency:** requires `Qg_tavg.nc` from Job A as a spatial template and mask.

<!-- TODO: document how to pass the water year argument to these SLURM scripts -->
<!-- TODO: clarify whether these are submitted with explicit dependencies (--dependency=afterok) or run manually in sequence -->

## Phase 2 — Run Blender

### Submit SLURM jobs for a full water-year run

From the `Github` directory:
```bash
cd /discover/nobackup/projects/coressd/Github
julia verse/Julia/submit_slurm_v2.jl 2015
```

This generates and submits approximately **500 SLURM jobs**, each processing one horizontal slice of the continent.

SLURM job outputs are saved to:
```
/discover/nobackup/projects/coressd/Github/slurm_jobs/2015/.out/
```

Slice output NetCDF files are written to:
```
/discover/nobackup/projects/coressd/Blender/Runs/WY2015/temp_nc/
```

### Post-processing: Combine slices into continental output

After all SLURM jobs complete, merge the slice files:
```bash
cd /discover/nobackup/projects/coressd/Github/Slurm_Blender
sbatch c_combine_nc_files.sh 2015
```

Final output files are written to:
```
/discover/nobackup/projects/coressd/Blender/Runs/WY2015/outputs/
```

> **Cleanup:** After successful merge, the `temp_nc/` directory can be deleted.

<!-- TODO: document expected output filenames and variables in the final outputs/ directory -->
<!-- TODO: document any post-processing quality checks or validation steps -->

## Edge Cases / Testing Modes

### Pixel mode

Runs Blender for a specified list of individual pixels (useful for debugging):

```bash
julia verse/Julia/call_Blender.jl "pixel/pixel_WY2015" 200 200 2
```

Requires: `verse/data/pixels.csv` (CSV of pixel row/column locations).

### Watershed mode

Runs Blender for a bounding box around a specific watershed:

```bash
julia verse/Julia/call_Blender.jl "wshed/wshed_WY2015" 100 100 2
```

Requires: `verse/data/wshed.csv` (CSV of watershed bounding boxes). The watershed index is 1-based (Julia convention).

<!-- TODO: document wshed.csv and pixels.csv format -->
<!-- TODO: document the 4th argument (the `2` in the examples above) -->
