# Output Files

## Grid Specification

All output files are on the same MODIS Sinusoidal grid as the input files (see [Directory Structure](discover_structure.md)). Each file has dimensions:
```
11700 x 4200 x 365
```

corresponding to the full North American domain at 1 km resolution for a single water year (October 1 – September 30).

## Primary Outputs

The two science outputs are `SWE.nc` and `Precip.nc`:

| File | Description | Approximate Size |
|---|---|---|
| `SWE.nc` | Daily snow water equivalent [m] | ~16 GB |
| `Precip.nc` | Daily accumulated snowfall [m] | ~27 GB |

These are written to:
```
/discover/nobackup/projects/coressd/Blender/Runs/WYxxxx/outputs/
```

## Other Output Files

The remaining files in the `outputs/` directory (`G.nc`, `Gmelt.nc`, `Gpv.nc`, `Gmeltpv.nc`, `SWEpv.nc`, `Upv.nc`, `Us.nc`) are either placeholders or non-functional outputs carried over from the original Blender manuscript. These should be removed in a future cleanup of the pipeline.

> **Note:** `Gmelt.nc` is the same size as `Precip.nc` (~27 GB) because it was used as a duplicate write target during testing and was never disabled. It does not contain meaningful output.

<!-- TODO: remove placeholder output files from pipeline and update this page accordingly -->