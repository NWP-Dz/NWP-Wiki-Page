## Introduction

gl ( as in **g**rib**l**ist ) is a multi purpose tool for file manipulation and conversion. It uses ECMWF's   [ecCodes](https://confluence.ecmwf.int//display/ECC/What+is+ecCodes) library, and can be compiled with and without support for HARMONIE FA/LFI or NETCDF files.

```bash
 USAGE: gl file [-n namelist_file] [-o output_file] -[lfgmicp(nc)sdtq] [-lbc CONF]

 gl [-f] file, list the content of a file, -f for FA/lfi files  
 -c    : Convert a FA/lfi file to grib ( -f implicit )          
 -p    : Convert a FA file to grib output without extension zone
         (-c and -f implicit )                                  
 -nc   : Convert a FA/lfi file to NetCDF ( -f implicit )        
 -musc : Convert a MUSC FA file ASCII ( -c implicit )           
 -lbc ARG : Convert a CONF file to HARMONIE input               
            where CONF is ifs or hir as in ECMWF/HIRLAM data    
         climate_aladin assumed available                       
 -d    : Together with -lbc it gives a (bogus) NH boundary file   
         climate_aladin assumed available                       
 -s    : Work as silent as possible                             
 -g    : Prints ksec/cadre/lfi info                             
 -m    : Prints min,mean,max of the fields                      
 -i    : Prints the namelist options (useless)                  
 -tp   : Prints the GRIB parameter usage                        
 -t    : Prints the FA/lfi/GRIB table (useful)                  
 -wa   : Prints the atmosphere FA/NETCDF/GRIB table in wiki fmt 
 -ws   : Prints the surfex FA/NETCDF/GRIB table in wiki fmt     
 -q    : Cross check the FA/lfi/GRIB table (try)                
 -pl X : Give polster_projlat in degrees                        

 gl file -n namelist_file : interpolates file according to      
                            namelist_file                       
 gl -n namelist_file : creates an empty domain according to     
                       specifications in namelist_file          
 -igd  : Set lignore_duplicates=T                               
 -igs  : Set lignore_shortname=T. Use indicatorOfParameter      
             instead of shortName for selection                 

```

## ecCodes definition tables

Since ecCodes has replaced `grib_api` as the ECMWF primary software package to handle GRIB, we will hereafter only refer to ecCodes but same similar settings applies for `grib_api` as well. With the change to ecCodes we heavily rely on the shortName key for identification. To get the correct connection between the shortnames and the GRIB1/GRIB2 identifiers we have defined specific tables for harmonie. These tables can be found in `/util/gl/definitions`. To use these tables you have to define the `ECCODES_DEFINITION_PATH` environment variable as 

```bash
export ECCODES_DEFINITION_PATH=SOME_PATH/gl/definitions:PATH_TO_YOUR_ECCODES_INSTALLATION
```

If this is not set correctly the interpretation of the fields may be wrong.

## GRIB/FA/LFI file listing

Listing of GRIB/ASIMOF/FA/LFI files.

```bash
 gl [-l] [-f] [-m] [-g] FILE
```

where FILE is in GRIB/ASIMOF/FA/LFI format

| Option  |  Description           |
| :--- | :---                      |
| `-l`   | input format is LFI       |
| `-f`   | input format is FA        |
|      | `-l` and `-f` are equivalent  |
| `-g`   | print GRIB/FA/LFI header  |
| `-m`   | print min/mean/max values |

## GRIB/FA/LFI file conversion

### Output to GRIB1

```bash
gl [-c] [-p] FILE [ -o OUTPUT_FILE] [ -n NAMELIST_FILE]
```

where 

|     |                                                                                          |
| --- | ---                                                                                      |
| -c  | converts the full field (including extension zone) from FA to GRIB1                      |
| -p  | converts field excluding the extension zone ("p" as in physical domain) from FA to GRIB1 |

The FA/LFI to GRIB mapping is done in a table defined by a `util/gl/inc/trans_tab.h`

To view the table:

```bash
gl -t
gl -tp
```

To check for duplicates in the table:

```bash
gl -q
```

The translation from FA/LFI to GRIB1 can be changed through a namelist like this one:

```bash
  &naminterp
    user_trans%full_name ='CLSTEMPERATURE',
    user_trans%t2v       = 253,
    user_trans%pid       = 123,
    user_trans%levtype   = 'heigthAboveGround',
    user_trans%level     = 002,
    user_trans%tri       = 000,
  /
```

or for the case where the level number is included in the FA name

```bash
  &naminterp
    user_trans%full_name='SNNNEZDIAG01',
    user_trans%cpar='S'
    user_trans%ctyp='EZDIAG01',
    ...
  /
```


Conversion can be refined to convert a selection of fields. Below is and example that will write out 
* T (shortname='t',pid=011), u (shortname='u',pid=033) andv (shortname='v',pid=034) on all (level=-1) model levels (levtype='hybrid')
* T (shortname='t',pid=011) at 2m (lll=2) above the ground (levtype='heightAboveGround') [T2m]
* Total precipitation (shortname='tp',pid=061,levtype='heightAboveGround',level=000)

```bash
  &naminterp
   readkey%shortname=   't',     'u',     'v',                't',               'tp',               'fg',
   readkey%levtype='hybrid','hybrid','hybrid','heightAboveGround','heightAboveGround','heightAboveGround',
   readkey%level=        -1,      -1,      -1,                  2,                  0,                 10,
   readkey%tri =          0,       0,       0,                  0,                  4,                  2,
  /
```
where 
* shortname is the ecCodes shortname of the parameter 
* levtype is the ecCodes level type
* level is the GRIB level
* tri means timeRangeIndicator and is set to distinguish between instantaneous, accumulated and min/max values.

The first three ones are well known to most users. The time range indicator is used in ALADIN/AROME to distinguish between instantaneous and accumulated fields.
We can also pick variables using their FA/lfi name:

```bash
  &naminterp
    readkey%faname = 'SPECSURFGEOP','SNNNTEMPERATURE',
  /
```

Where `SNNNTEMPERATURE` means that we picks all levels.

Fields can be excluded from the conversion by name

```bash
  &naminterp
    exclkey%faname = 'SNNNTEMPERATURE'
  /
```

### Output to GRIB2
 To get GRIB2 files the format has to be set in the namelist as 

```bash
  &naminterp
    output_format = 'GRIB2'
  /
```

 The conversion from FA to GRIB2 is done in gl via the ecCodes tables. All translations are defined in `util/gl/scr/harmonie_grib1_2_grib2.pm` where we find all settings required to specify a parameter in GRIB1 and GRIB2.

```bash

  tmax => {
   editionNumber=> '2',
   comment=> 'Maximum temperature',
   discipline=> '0',
   indicatorOfParameter=> '15',
   paramId=> '253015',
   parameterCategory=> '0',
   parameterNumber=> '0',
   shortName=> 'tmax',
   table2Version=> '253',
   typeOfStatisticalProcessing=> '2',
   units=> 'K',
  },

```

 To create ecCodes tables from this file run
 
```bash
   cd gl/scr
   ./gen_tables.pl aladin/arome_grib1_2_grib2
```

 and copy the grib1/grib2 directories to gl/definitions.

 Note that there are no GRIB2 transations yet defined for the SURFEX fields!
