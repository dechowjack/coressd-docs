# Algorithm

The Blender algorithm is implemented in Julia and lives in the [`bidhya/verse`](https://github.com/bidhya/verse) GitHub repository. Key files:

| File | Description |
|---|---|
| `Julia/Estimate_v2.jl` | Core estimation / optimization logic |
| `Julia/call_Blender_v2.jl` | Entry point for a single Blender run (one slice/watershed) |
| `Julia/submit_slurm_v2.jl` | Generates and submits ~500 SLURM jobs for a full water-year run |
| `Julia/combine_nc_files.jl` | Merges per-slice outputs into continental NetCDF |
| `Python/Extract_LIS.py` | Extracts and formats LIS outputs as Blender inputs |
| `Python/process_modis_cgf.py` | Processes and clips MODIS CGF SCF to match LIS grid |

See [Running Blender](running_blender.md) for execution instructions.


`filename_v2.jl` indicates the version of the algorithm that runs with the precipitation scalar.