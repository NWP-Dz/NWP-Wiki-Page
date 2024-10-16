# RESTOR

---

## About

"RESTOR" allows users to perform reanalysis based on "ALADIN" and "AROME" models for past situations, following user requests (forecasting, climatology, external clients, etc.).

This directory is composed of three sub-directories:

- **ALADIN**: For reanalysis using the ALADIN model.
- **AROME**: For reanalysis using the AROME model.
- **grads**: Contains the necessary scripts for generating maps.

The scripts and programs are designed in a portable format, meaning you can copy the "RESTOR" directory into your home directory and run the reanalysis without needing to modify any environment variables.

## Before the First Use

- Insert your access password to the supercomputer in the script "`****_GET_DATA.sh`" at line 34, replacing `'HPC_PASSWORD'`.
- Install the `sshpass` command using: 
```bash
   apt-get install sshpass
```

## Running the Processing Scripts

To run the scripts, update the date configuration file "`date.config`" with the desired date parameters (year, month, day, etc.).

For both models (ALADIN and AROME), there is a main script that automatically executes four (04) sub-scripts:

### For ALADIN

Run the following command:
```bash
    ./RUN-RESTOR-ALADIN.sh
```
This script executes the following sub-scripts in order:
1. `./0-ALAD_GET_DATA.sh`
2. `./1-ALAD_EDF_JOB.sh`
3. `./2-ALAD_EXE_JOB.sh`
4. `./3-ALAD_GRADS_JOB.sh`

### For AROME

Run the following command:
```bash
    ./RUN-RESTOR-AROME.sh
```
This script executes the following sub-scripts in order:
1. `./0-ARO_GET_DATA.sh`
2. `./1-ARO_EDF_JOB.sh`
3. `./2-ARO_EXE_JOB.sh`
4. `./3-ARO_GRADS_JOB.sh`

## Important

Please check the log files (`log.aladin` and `log.arome`) after running the post-processing scripts for each model to ensure that the scripts have executed correctly.