### CliMake

------------------------------------------------------------------

The goal of this package is to provide a simple, flexible and efficient set of files and scripts 
in order to make PGD and clim files in various domains.
It's a modernization of Françoise Taillefer base scripts, using the GENV/GGET GCO tools. 

Credits to:
Suzana Panezics, Alexandre Mary, Florian Suzat

If you have any problem, question or proposition please contact
florian.suzat@meteo.fr  

------------------------------------------------------------------

To use this script, you need access to MF machine then please follow the following steps:

_ clone the CLIMAKE directory. From supercomputer 'beaufix', go to the directory where you want CLIMAKE 
script system launch the command:
```bash
'~suzat/SAVE/cloneClimake'
```
This directory contains:
 * this README file
 * a directory 'geometries' containing only the geometry related namelist blocks for each domain 
 (both for PGD and 923). 
 You can create your own file there, but also you can send us those files so that we can archive it. 
 * a directory 'namelists' containing the PGD and 923 namelists files. Some examples are given.
 Geometry related information have been removed from thoses files.
 * a directory 'scripts' containing the various scripts files needed for this configuration.
Normally, you should not need to edit them, but if you know what you are doing, you can!
There are ordered in 4 steps, as they are named. 
 * the 'climake' main script that take arguments, make some controls and launch the process. 
 * the 'postpromake' script that works like 'climake', to do lat/lon postprocessing clim files

------------------------------------------------------------------

GENV (GCO) CYCLE:

First of all, you have to choose what we call a GENV cycle.
It's a tag corresponding to a version of the set of files needed for the PGD/923 configuration.
To know the last cycle ( the more recent ) you can launch on supercomputer the command
 ```bash
'/home/mf/dp/marp/martinezs/public/bin/genv oper-clim'  ---> corresponding to the operational suite
```
For cy43 the official GENV cycle is 'cy43t2_clim-op2.02'

This command will give you a list of key/value.
The CYCLE can be read after the key 'CYCLE_NAME'.
It contains all the resources useful for the clim configuration (input files, namelists, binaries, etc...).

For instance, to get the HWSD_SAND database associated to the GENV cycle cy43t2_clim-op1.01, just call:
```bash
'/home/mf/dp/marp/martinezs/public/bin/gget hwsd.sand_v2.01.tgz'
```
to get the ECOCLIMAP_COVERS_PARAM call:
```bash
'/home/mf/dp/marp/martinezs/public/bin/gget ecoclimap.covers.param.05.tgz'
```
etc...
This mecanism avoid to set in the script hard coded links to files, and historisation 
(because GENV/GGET check the checksums of files, and ensure they will not be modified)

If you don't know what CYCLE to choose, you can use the last operational suite given by 
```bash
'/home/mf/dp/marp/martinezs/public/bin/genv oper-clim'
```
here is a list of old genv cycles related to clims...
2017/03/09	cy42_clim-op2.01 
2016/11/08	al41t1_clim-op2.02
2016/11/08	al42_clim-op2.01
2016/05/03	al41t1_clim-op2.01
2015/09/23	al41t1_clim-op1.03
2015/08/27	al41t1_clim-op1.01
2015/06/10	al40_clim-op2.04
2015/06/02	al40_clim-op2.03
2015/01/14	al38t1_clim-op3.01
2014/09/29	al40_clim-op1.05
2014/09/22	al40_clim-op1.04
2014/07/31	al40_clim-op1.03
2014/07/09	al40_clim-op1.02
2014/07/09	al38t1_clim-op2.02
2014/04/07	al40_clim-op1.01
2014/04/07	cy40_clim-op1.01
2013/09/16	al38t1_clim-op2.01
to find that you can go to http://gco.meteo.fr/   --> history of cycle put in date field '201*' then update and grep with ctrl +f 'clim'

------------------------------------------------------------------

GEOMETRY:

They you have to choose 2 geometry namelists (one for the PGD, one for the 923) 
either among the existing ones on 'geometries' directory or created by you 
(let us know if you want that we archive it).

In 923, there is two possibility: 
 - orography computed in 2 steps  (normal clim configuration)
 - orography computed in one step (for example for TELECOM domains)
 
The behaviour for orography steps depends of what you give in arguments on 'climake' command line or config files.
   


------------------------------------------------------------------

LAUNCH

since Version 1.21 CLIMAKE can be launched in 2 modes : with config file or interactively
* in the first case the command is :
```bash
    ./climake -c config_file
```
you can edit the file config_example.conf to create the file you need
* you can either launch CLIMAKE with -i option
```bash    
    ./climake -i
```
you will be prompted for the various options to choose...

This will use PGD and MASTER binary from the GENV cycle

But
* You can use your own PGD (resp. MASTER) binary by setting environment variable CUSTOM_PGD (resp. CUSTOM_MASTER)
* You can use a custom PGD namelist by replacing the first  'GCO' argument by a path to a namelist
(without geometry relateds block). You can start from ./namelist/PGD/GENERIC.nam which is the standard PGD cy43t2 namelist

* You can use a custom 923 namelist by replacing the second 'GCO' argument by a path to a namelist
(without geometry relateds block). You can start from ./namelist/923/GENERIC.nam which is the standard 923 cy43t2 namelist

Climake will do some check and some pre calculations.
You can choose an automatic calculation of NSMAX/NMSMAX from NDGLG and NDLON or manual fixing of thoses values.
Before starting the job, climake will produce a sort a short summary, that you have to confirm.

------------------------------------------------------------------
RESULTS

Return listings of the scripts will arrive into the CLIMAKE directory.
You can get the working directory in those return listings (by greping Workdir).
You can visit this workdir to show in real time what is happening.
In the workdir you will find
_all the input files needed
_all the listings of each step (called listing.step_XXX)
_clim and pgd being constructed
Each step in run in independents directory (build_pgd, orography, finalize_orography, const_physiography,...)
IMPORTANT: to save space in the HOME directory, the WORKDIR is located in a directory that will be cleaned automatically.
If you want to keep it available you must save this $WORKDIR (by tar-gziping it and achiving it on Hendrix for example).

Files will finally be produced in the $OUTPUTDIR directory.
An extra step producing a html report page, and the plots (every fields of every files) will be produced in this directory.

This last step also rewrite the SUFGEOPOTENTIAL field of the clim inside the PGD SFX.ZS field.

HAPPY CLIMAKING!


------------------------------------------------------------------
POSTPROMAKE 

Postpromake work as climake. There is 2 interactive mode -f (file mode) and -a (automatic mode) and 1 config file mode -c as climake.
-a and -f mode are available with option -c.
See examples :

for file mode (you need to specify geometry namelist delta)
```bash
./postpromake -c postpromake_config_filemode_example.conf 
```

for automatic mode (you need to specify inputs then epygram will calculate for you the geometries namelistsa)
```bash
./postpromake -c postpromake_config_auto_example.conf 
```
In automatic mode, postpromake takes as input:
_ the resolution in degree of the grid 
_ the bounds of the lat lon square
```bash
             lonmin,latmin (ELON1,ELAT1)                           lonmax,latmin (ELON2,ELAT1)
                       x --------------------------------------------------- x
                       |                                                     |
                       |                                                     |
                       |                                                     |
                       |                                                     |
                       |                                                     |
                       |                                                     |
                       x --------------------------------------------------- x
             lonmin,latmax (ELON1,ELAT2)                           lonmax,latmax (ELON2,ELAT2)
```
the program will calculate and generate the geometry files (as GMAP do in Olive) : it uses vortex and epygram, 2 python GMAP toolbox.
These calculations are done in the step 1 (after the genv file prefetching)
_ then postpromake uses the same scripts as climake. There is only one orography step and no spectral at all (thats why there is a specific namelist delta inside "stuff" directory)

For the moment epygram is not able to handle lat/lon geometries that have different resolutions in Lat and Lon... It may be available soon....

