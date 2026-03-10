## Basic Information
The *Co*ld *Re*gions *S*nowfall and *S*nowpack *D*ataset (CoReSSD) is a 1 km snowfall and snow water equivalent dataset. This dataset was produced with funding from NASA Grant #80NSSC22K0585. CoReSSD relies on the `Blender` algorithm (Dechow et al., 2026, Water Resources Research). Blender is a non-linear optimization technique to recompute the modeled mass and energy balance of the snowpack, constrained by remotely sensed snow cover fraction. Blender requires a prior estimate, i.e. initial conditions, of the snowpack mass and energy balance. The original Blender algorithm paper can be found below.

<!-- TODO: confirm full product name expansion and tagline with PI -->

##  CoReSSD Product Specifications
- `TEMPORAL COVERAGE`:	 	    OCT 1 2000 – SEP 30 2021
- `TEMPORAL RESOLUTION`:		DAILY
- `SPATIAL COVERAGE`: 		    NORTH AMERICA [-169W 72N -52E 30S]
- `SPATIAL RESOLUTION`: 		1 KM
- `DATA FIELD 1`:	 		    SNOW WATER EQUIVALENT [m]
- `DATA FIELD 2`:	 			ACCUMULATED SNOWFALL [m]

The CoReSSD product is gridded at 1 km on the MODIS Sinusoidal Grid. The `MODIS_Sinusoidal.prj` file that contains the projection WKT is available in the top-level CoReSSD repository.

<!-- TODO: add specifics to projection -->
## Input Data
Blender requires 5 input fields, plus an optional 6th field. The first four inputs are the modeled components of the snowpack mass and energy balance, and the 5th is the remotely sensed snow cover fraction (SCF). The optional 6th field, the precipitation scalar, is used when either the a. SCF product is much higher resolution than the modeled fields, or b. Mass inputs are not well distributed spatially. This can occur in nominally high resolution runs (< 3 km) with very coarse meteorological forcings. The second scenario is the case in the production of the CoReSSD product. 

### Modeled Inputs

- `Snowf_tavg`:	 	snowfall rate [kg m-2 s-1]
- `SWE_tavg`:		snow water equivalent [kg m-2]
- `Qg_tavg`: 		downward soil heat flux [W m-2]
- `Tair_f_tavg`: 	air temperature [deg K]

The `tavg` field suffix indicates the field is a “time-average value” over the timestep. In the production of the CoReSSD prior, the timestep was `daily`. 

### Observed Inputs

- `SCF`:	 pixel snow cover fraction [dimensionless]

### Optional Inputs

- `precip_scalar`:	 precipitation scalar value [dimensionless]

The precipitation scalar is an optional input to assist in downscaling the mass inputs. Further documentation for this input is available in Dechow et al., 2026. A generic version of the downscaling algorithm is available in the pyDRADS GitHub repository [link here]. If an input is not provided, the Blender algorithm fills a dummy value of 1, which effectively turns this option off.


## Related Resources

- Algorithm description: [Algorithm](algorithm.md)
- Prior generation: [Prior Generation](prior_generation.md)
- Intermediary processing: [Intermediary Processing](intermediary_processing.md)
- Dechow et al. (2026), *Water Resources Research* <!-- TODO: add DOI/link when published -->
