# File formats in HARMONIE

## Introduction

The HARMONIE system reads and writes a number of different formats. 

## FA files

 Default internal format input/output for HARMONIE for gridpoint, spectral and SURFEX data. GRIB is used as a way to pack data, but the grib record cannot be used as such.

 * The header contains information about model domain, projection, spectral truncation, extension zone, boundary zone, vertical levels. 
 * Only one date/time per file.
 * FA routines are found under `ifsaux/fa`
 * List or convert a file with [`gl`](@ref gl)
 * Other listing tool [PINUTS](http://www.cnrm.meteo.fr/gmapdoc/spip.php?page=recherche&recherche=PINUTS)

 [Read more](http://www.cnrm.meteo.fr/gmapdoc/spip.php?page=recherche&recherche=FA+)

## NETCDF

 In climate mode all FA files may converted to NETCDF after the forecast run. For the conversion between FA names and NETCDF parameters check `util/gl/inc/nc_tab.h`.

 * For the manipulation and listing of NETCDF files we refer to standard NETCDF tools.
 * NETCDF is also used as output data from some SURFEX tools.




