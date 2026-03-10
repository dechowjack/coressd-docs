# Known Issues and To-Do

This page tracks known issues, limitations, and outstanding tasks for the CoReSSD product and processing pipeline.

<!-- TODO: expand this list with confirmed issues from the team -->

## Known Issues

- **`temp_nc/` disk usage:** The per-slice Blender output files in `Blender/Runs/WYxxxx/temp_nc/` can consume significant disk space. These must be manually deleted after the continental merge step completes successfully.

- **NetCDF metadata after MATLAB smoothing:** MATLAB does not write fully CF-compliant NetCDF attributes. The `fix_smoothed_attrs.sh` script corrects these, but the specific attributes requiring fixes should be documented more thoroughly (see [Intermediary Processing](intermediary_processing.md)).


## To-Do

- [ ] Document `functions/` subdirectory in `csd_dev` (see [Function Reference](functions.md))
- [ ] Add DOI and full citation for Dechow et al. (2026) WRR once published
- [ ] Confirm and document output coordinate reference system
- [ ] Document Gaussian smoother kernel parameters
- [ ] Document NCO module requirements on Discover for `fix_smoothed_attrs.sh`
- [ ] Document expected output file structure and variable attributes for final Blender outputs
- [ ] Add validation / QC guidance for inspecting outputs

