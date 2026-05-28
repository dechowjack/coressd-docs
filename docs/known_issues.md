# Known Issues and To-Do

This page tracks known issues, limitations, and outstanding tasks for the CoReSSD product and processing pipeline.

## Known Issues

- **`temp_nc/` disk usage:** The per-slice Blender output files in `Blender/Runs/WYxxxx/temp_nc/` can consume significant disk space. These must be manually deleted after the continental merge step completes successfully.

- **Placeholder output files:** Several files in `Blender/Runs/WYxxxx/outputs/` are carried over from the original Blender workflow and are not primary CoReSSD science products. See [Output Files](output_files.md) for the current interpretation.


## To-Do

- [ ] Document `functions/` subdirectory in `csd_dev` (see [Function Reference](functions.md))
- [ ] Add DOI and full citation for Dechow et al. (2026) WRR once published
- [ ] Confirm and document output coordinate reference system
- [ ] Document Gaussian smoother kernel parameters
- [ ] Add validation / QC guidance for inspecting outputs
