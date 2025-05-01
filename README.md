# code_CLMU_traffic

## Introduction

This repository is supplementary to the manuscript "Sun, Y., Oleson, K. W., & Zheng, Z. (2025). Modeling urban traffic flux in Community Earth System Model".

The objectives of this project are:

- Modify the CESM source code to incorporate an urban traffic heat flux module for quantifying anthropogenic heat flux (AHF);
- Validate model performance with the new traffic module at two sites;
- Examine traffic-induced thermal impacts on the urban environment and building energy use.

## Script and data

### [1_code_modification](./1_code_modification)

The standard source code comes from [CTSM](https://github.com/ESCOMP/CTSM), with the release tag: [ctsm5.3.024](https://github.com/ESCOMP/CTSM/tree/ctsm5.3.024). See modified code lines labeled with **!YS**.

- Create a new module to read time-varying traffic data and provide traffic-related functions:
  - [src/biogeophys/UrbanVehicleType.F90](./1_code_modification/src/biogeophys/UrbanVehicleType.F90)

- Calculate the impervious road width and number of lanes:
  - [src/biogeophys/UrbanParamsType.F90](./1_code_modification/src/biogeophys/UrbanParamsType.F90)

- Pass traffic inputs to compute the traffic heat flux:
  - [src/biogeophys/EnergyFluxType.F90](./1_code_modification/src/biogeophys/EnergyFluxType.F90)
  - [src/biogeophys/UrbanFluxesMod.F90](./1_code_modification/src/biogeophys/UrbanFluxesMod.F90)

- Add the module in the model initialization and computation processes:
  - [src/main/clm_instMod.F90](./1_code_modification/src/main/clm_instMod.F90)
  - [src/main/clm_driver.F90](./1_code_modification/src/main/clm_driver.F90)
- Add time-varying input data to the namelists:
  - [bld/CLMBuildNamelist.pm](./1_code_modification/bld/CLMBuildNamelist.pm)
  - [bld/namelist_files/namelist_defaults_ctsm.xml](./1_code_modification/bld/namelist_files/namelist_defaults_ctsm.xml)
  - [bld/namelist_files/namelist_definition_ctsm.xml](./1_code_modification/bld/namelist_files/namelist_definition_ctsm.xml)

### [2_single_point_simulations](./2_single_point_simulations)

We conducted a pair of single-point simulations (CNTL and TRAF) at the [Capitole of Toulouse, France (FR-Capitole)](./2_single_point_simulations/FR-Capitole) and [Manchester, UK (UK-Manchester)](./2_single_point_simulations/UK-Manchester).

- FR-Capitole
  - [SourceMods](./2_single_point_simulations/FR-Capitole/SourceMods): code used at FR-Capitole simulations with additional modifications to use local parameters. 
  - [datm_files](./2_single_point_simulations/FR-Capitole/datm_files/): atmospheric forcing data, derived from the [Urban-PLUMBER](https://urban-plumber.github.io/).
  - [input_files](./2_single_point_simulations/FR-Capitole/input_files): surface data, derived from the [Urban-PLUMBER](https://urban-plumber.github.io/) and [UTC19 traffic dataset](https://utd19.ethz.ch/).
- UK-Manchester
  - [SourceMods](./2_single_point_simulations/UK-Manchester/SourceMods): code used at UK-Manchester simulations.
  - [datm_files](./2_single_point_simulations/UK-Manchester/datm_files/): atmospheric forcing data from bias-corrected [ERA5-Land reanalysis data](https://cds.climate.copernicus.eu/datasets/reanalysis-era5-land).
  - [input_files](./2_single_point_simulations/UK-Manchester/datm_files/): surface data from the [CTSM's default land surface dataset](https://svn-ccsm-inputdata.cgd.ucar.edu/trunk/inputdata/lnd/clm2/surfdata_esmf/ctsm5.3.0/) and [Transport for Greater Manchester (TfGM)](https://tfgm.com/)

### [3_simulation_output_analysis](./3_simulation_output_analysis)

The scripts listed below are used to visualize two sites with corresponding traffic diurnal cycles.

| Num. | Subject                                                      | Data process                                                 | Visualization                                                |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 3.1  | [Metadata at FR-Capitole](./3_simulation_output_analysis/3.1_FR-Capitole_metadata) | Using [Export.ipynb](./3_simulation_output_analysis/3.1_FR-Capitole_metadata/Export.ipynb) to get the diurnal mean vehicle volume | [Figure.ipynb](./3_simulation_output_analysis/3.1_FR-Capitole_metadata/Figure.ipynb) |
| 3.2  | [Metadata_at_UK-Manchester](./3_simulation_output_analysis/3.2_UK-Manchester_metadata) | Using [Export.ipynb](/3_simulation_output_analysis/3.2_UK-Manchester_metadata/Export.ipynb) to get the diurnal mean vehicle volume | [Figure.ipynb](./3_simulation_output_analysis/3.2_UK-Manchester_metadata/Figure.ipynb) |

The scripts listed below are used for processing CNTL and TRAF simulation output and visualization.

| Num. | Subject                                                      | Output data process                                          | Visualization                                                |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 3.3  | [Model_validation_at_FR-Capitole](./3_simulation_output_analysis/3.3_FR-Capitole_model_validation/) | Use [Export.ipynb](./3_simulation_output_analysis/2.3_FR-Capitole_model_validation/Export.ipynb) to get 7-day mean and hourly mean variables in comparison with observations | [Figure.ipynb](./3_simulation_output_analysis/3.3_FR-Capitole_model_validation/Figure.ipynb) |
| 3.4  | [Anthropoegnic_heat_at_FR-Capitole](./3_simulation_output_analysis/3.4_FR-Capitole_ahf) | Use [Export.ipynb](./3_simulation_output_analysis/3.4_FR-Capitole_ahf/Export.ipynb) to get 7-day mean and hourly mean variables related to AHF | [Figure.ipynb](./3_simulation_output_analysis/3.4_FR-Capitole_ahf/Figure.ipynb) |
| 3.5  | [Model_validation_at_UK-Manchester](./3_simulation_output_analysis/3.5_UK-Manchester_model_validation/) | Use [Export.ipynb](./3_simulation_output_analysis/3.5_UK-Manchester_model_validation/Export.ipynb) to get 7-day mean and hourly mean variables in comparison with observations | [Figure.ipynb](./3_simulation_output_analysis/3.5_UK-Manchester_model_validation/Figure.ipynb) |
| 3.6  | [Heat_stress_at_UK-Manchester](./3_simulation_output_analysis/3.6_UK-Manchester_heat_stress/) | Use [Export.ipynb](./3_simulation_output_analysis/3.6_UK-Manchester_heat_stress/Export.ipynb) to get heat stress indices | [Figure.ipynb](./3_simulation_output_analysis/3.6_UK-Manchester_heat_stress/Figure.ipynb) |
| 3.7  | [Compare_temperatures](./3_simulation_output_analysis/3.7_compare_temperatures/) | Use [Export.ipynb](./3_simulation_output_analysis/3.7_compare_temperatures/Export.ipynb) to get the difference in temperature between TRAF and CNTL simulations | [Figure.ipynb](./3_simulation_output_analysis/3.7_compare_temperatures/Figure.ipynb) |
| 3.8  | [Model sensitivity](./3_simulation_output_analysis/3.8_model_sensitivity) | Use [Export.ipynb](./3_simulation_output_analysis/3.8_model_sensitivity/Export.ipynb) to export Taylor Diagram metrics | [Figure.ipynb](./3_simulation_output_analysis/3.8_model_sensitivity/Figure.ipynb) |

### [4_illustration](./4_illustration)

The figures listed below are used to illustrate the details of the model workflow and mechanism.

| Subject                                                      | Visualization |
| ------------------------------------------------------------ | ------------- |
| Timeline of incorporating AHF in global simulation           | [Figure]()    |
| Workflow of incorporating urban traffic modeling in CTSM     | [Figure]()    |
| Biogeophysical mechanism of traffic-induced thermal effects  | [Figure]()    |
| Traffic-induced changes in heat flux and temperatures at FR-Capitole | [Figure]()    |
| Traffic-induced changes in heat flux and temperatures at UK-Manchester | [Figure]()    |
| Community Land Model                                         | [Figure]()    |



### 5_sumplimentary_materials