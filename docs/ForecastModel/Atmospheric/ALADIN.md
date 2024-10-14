# [Parameter list and GRIB definitions](@id outputlist) 

## HARMONIE system output

The HARMONIE system writes its primary output, in FA format, to the upper air history files `ICMSHHARM+llll` and the SURFEX history files `ICMSHHARM+llll.sfx`, where `HARM` is the four-character experiment identifier set in the configuration file `config_exp.h`, and `llll` is normally the current timestep in hours. The files are designed to be complete snapshots of respective model state described by the system for a particular time point. In addition more model output including post-processing/diagnostic fields can be written out during the forecast model integration, such as those model diagnostics or pressure level diagnostics, also in FA format, as `PFHARMDOMAIN+llll`. The FA files can be considered to be internal format files. All of them can be converted to GRIB files during the run for external usage. The name convention is as follows:
