# Output Files

## Grid Specification

All output files are on the same North American 1 km grid as the Blender input files (see [Directory Structure](discover_structure.md)). Each file has dimensions:
```
x = 11700
y = 4200
ti = 365 or 366
```

The `ti` dimension is daily and spans one water year, from October 1 through September 30. Leap-year water years contain 366 daily timesteps.

## Primary Outputs

The two science outputs are `SWE.nc` and `Precip.nc`:

| File | Description | Approximate Size |
|---|---|---|
| `SWE.nc` | Daily snow water equivalent [m] | ~16 GB |
| `Precip.nc` | Daily precipitation water-equivalent rate [m day-1] | ~27 GB |

These are written to:
```
/discover/nobackup/projects/coressd/Blender/Runs/WYxxxx/outputs/
```

## Expected NetCDF Metadata

The publication files should carry the following science-variable metadata:

| File | Variable | Units | Long name |
|---|---|---|---|
| `SWE.nc` | `SWE` | `m` | `snow water equivalent` |
| `Precip.nc` | `Precip` | `m day-1` | `daily precipitation water-equivalent rate` |

The same `units` and `long_name` attributes are also written as global attributes in each file. The science variables should not retain copied `standard_name`, `vmin`, or `vmax` attributes from template/input files.

The maintained metadata repair helper is:

```bash
/discover/nobackup/projects/coressd/csd_scripts/metadata/fix_output_netcdf_metadata.sh
```

Run it after the final yearly output files have been merged. The helper uses `ncatted`, attempts to load `nco/5.1.7` on Discover if needed, and writes before/after header snapshots to `Analysis/metadata_headers/`.

## Other Output Files

The remaining files in the `outputs/` directory (`G.nc`, `Gmelt.nc`, `Gpv.nc`, `Gmeltpv.nc`, `SWEpv.nc`, `Upv.nc`, `Us.nc`) are either placeholders or non-functional outputs carried over from the original Blender manuscript. These should be removed in a future cleanup of the pipeline.

> **Note:** `Gmelt.nc` is the same size as `Precip.nc` (~27 GB) because it was used as a duplicate write target during testing and was never disabled. It does not contain meaningful output.
