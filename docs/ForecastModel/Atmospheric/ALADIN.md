# Forecast Model 

## ALADIN

ALADIN (Limited Area Model Adaptation Dynamique INitialisation) is a limited-area version of ARPÈGE. It is a spectral model; its horizontal domain covers only a well-defined area, , so the fields are “bi-periodicised” to be made able to match with a spectral representation. The vertical coordinate is the same as the one of ARPEGE.
ALADIN is forced by using coupling files of ARPEGE. Most of the code is common to ALADIN and ARPEGE, but there are specific applications which require different parts of code. ALADIN currently uses the same package of physics as ARPEGE