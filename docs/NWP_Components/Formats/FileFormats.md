# File formats in HARMONIE

## Introduction

The HARMONIE system reads and writes a number of different formats. 

## FA files

 Default internal format input/output for gridpoint, spectral and SURFEX data. GRIB is used as a way to pack data, but the grib record cannot be used as such.

 * The header contains information about model domain, projection, spectral truncation, extension zone, boundary zone, vertical levels. 
 * Only one date/time per file.
 * FA routines are found under `ifsaux/fa`
 * List or convert a file with [Gl](../../Tools/Gl/gl.md)
 * Other listing tool [PINUTS](http://www.cnrm.meteo.fr/gmapdoc/spip.php?page=recherche&recherche=PINUTS)

 [Read more](http://www.cnrm.meteo.fr/gmapdoc/spip.php?page=recherche&recherche=FA+)

## GRIB/GRIB2

 All FA files may be converted to GRIB after the forecast run. For the conversion between FA names and GRIB parameters check [this link](https://hirlam.github.io/HarmonieSystemDocumentation/dev/ForecastModel/Outputlist/).

 * List or convert a GRIB file with [Gl](../../Tools/Gl/gl.md)




