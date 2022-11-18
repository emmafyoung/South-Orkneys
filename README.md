- - - -
# South Orkneys Oceanographic Model
NEMO regional configuration for the South Orkneys
- - - -

## Introduction
A regional oceanographic model for the South Orkney islands region in the southwest Atlantic has been developed to undertake analyses on the spatial and temporal variability in the regional oceanography, the key drivers of this variability, and the impact on the distribution of exploited species. The model development was funded by the UK Foreign, Commonwealth and Development Office. The underlying model system is NEMO (Nucleus for European Modelling of the Ocean; <https://www.nemo-ocean.eu/>), which has been widely adopted by the European scientific community for a range of modelling studies from regional to global scales. NEMO is a highly versatile modelling system, and includes a range of options for model parameterisation that enables good representation of regions characterised by complex bathymetry and precipitous shelf-edges.The simulation of sea-ice is included by coupling with the LIM3 sea-ice module.

## Model Overview

The South Orkneys regional model extends from 65°S, 64°W to 57.5°S, 35°W, with bathymetry derived from GEBCO2014 (Figure 1). The model has a horizontal resolution of 1/20° longitude by 1/40° latitude (~2.3 - 3km, varying with latitude) with variables arranged on an Arakawa C grid. The vertical dimension is represented by 75 levels arranged as partial-cell z-levels, and with variable cell depth such that vertical resolution is enhanced over the shallow shelf regions. The model is forced at the open boundaries with tides from a global tidal model (TPXO7.2), and with barotropic flows, sea surface height, temperature, salinity and sea-ice from a global application of NEMO-LIM2 at 1/12° horizontal resolution (ORCA0083-N06) provided by the National Oceanography Centre, Southampton. Atmospheric forcing is derived from reanalysis (DFS5.2). Climatological spatially-varying terrestrial freshwater input is derived from a combination of precipitation data from the DFS5.2 reanalysis, and Antarctic Peninsula glacier basin discharge data from the Regional Atmospheric Climate Model (RACMO; van Wessem et al., 2017). The model has been run for 1992-2012 with the following variables saved as 5-day mean fields: sea surface height, mixed layer depth, sea-ice concentration, sea-ice thickness, snow thickness, sea-ice velocities, 3D ocean temperature, 3D ocean salinity, 3D ocean velocities. Output data are available from Emma Young (eyoung@bas.ac.uk) by request.

![](SOmodel_bathy_4git_cropped.png)
#### Figure 1: Model bathymetry; black contours are at 500, 1000 and 2500 m.

## Installation instructions

The South Orkneys regional model was developed using NEMOv3.6 and XIOS2.0, and was run on the ARCHER high performance computer. The following instructions are specific to that combination of software and hardware.
* First, install the appropriate versions of NEMO and XIOS on ARCHER in your home directory. NEMOv3.6 is available to download from https://forge.ipsl.jussieu.fr/nemo . To obtain XIOS2.0, use the following command: 
svn co http://forge.ipsl.jussieu.fr/ioserver/svn/XIOS/branchs/xios-2.0
* Next, load netCDF and HDF5 libraries as well as Intel compilers. On ARCHER, the commands are:
  * module load cray-netcdf-hdf5parallel
  * module load cray-hdf5-parallel
  * module swap PrgEnv-cray PrgEnv-intel
* Put the file containing the compiler settings for NEMO (arch-XC_ARCHER_INTEL.fcm in directory ARCH) in the directory NEMOGCM/ARCH/.
* Put the files containing the compiler settings for XIOS (arch-XC30_ARCHER.fcm, arch-XC30_ARCHER.path and arch-XC30_ARCHER.env in directory ARCH) in the directory XIOS/ARCH/.
* Install XIOS by running the following command in the XIOS folder:
  * ./make_xios --prod --full --arch XC30_ARCHER
* Copy the whole XIOS directory to your work area, and edit the NEMO ‘arch’ file so the correct path to the XIOS directory in your work area is specified for %XIOS_HOME.
* Copy the XIOS executable (xios_server.exe) from the XIOS bin directory to NEMOGCM/CONFIG/GYRE_XIOS/EXP00
* Navigate to the directory NEMOGCM/CONFIG and create a baseline model configuration from the reference GYRE_XIOS case as follows:
  * ./makenemo -n SOice -m XC_ARCHER_INTEL -r GYRE_XIOS
* Replace file cpp_SOice.fcm in directory SOice with the version provided here (in directory CPP).
* Put configuration-specific versions of source code provided here (in directory MY_SRC) into directory SOice/MY_SRC.
* Put the following configuration-specific files (in directory EXP00) in the SOice/EXP00 directory:
  * bathy_meter.nc
  * coordinates.nc
  * coordinates.bdy.nc
  * namelist_cfg
  * namelist_ice_cfg
  * weights_dfs5.2_SOnemo_bilin.nc
* Create directory DATA in the SOice/EXP00 directory for external forcing data (open boundary forcing, atmospheric forcing, terrestrial freshwater inputs).
* Download the DRAKKAR Forcing Set (DFS) atmospheric reanalysis data for 1990 listed in the namelist_cfg file to the SOice/EXP00/DATA directory; if you are not able to access this through DRAKKAR, please contact Emma Young (eyoung@bas.ac.uk).
* Open boundary and terrestrial freshwater forcing are available on the BAS external ftp site (ftp://ftp.bas.ac.uk) in directory eyoung/SO/DATA. Your login should be username=anonymous with your full email address as password. Put downloaded forcing files in directory SOice/EXP00/DATA.
* Files containing example initial conditions (SOnemo_init_T_030190_06.nc and SOnemo_init_S_030190_06.nc) are also available on the BAS external ftp site (see above) in directory eyoung/SO/INIT. These should be placed in the SOice/EXP00 directory.
* Navigate to NEMOGCM/CONFIG/SOice/EXP00. Create a runscript or adapt the example files provided (runnemo.sh.1990 and run.SO.pbs.1990 in directory EXP00).
* Navigate to NEMOGCM/CONFIG, clean the configuration and recompile as follows:
  * ./makenemo -n SOice clean
  * ./makenemo -n SOice -m XC_ARCHER_INTEL
* Run the model:
  * qsub run.SO.pbs.1990

## Example model output
Figures showing example output for the model configuration and forcing data detailed here are shown in directory OUTPUT. The figures show 5-day mean surface temperatures and horizontal near-surface velocities (shading is current speed, velocity vectors are plotted every fifth grid cell) for 26-30 January 1990.

