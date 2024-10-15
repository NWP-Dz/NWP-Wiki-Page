# [Surface Data Assimilation Scheme: Canari ](@id canari)

## Introduction
(by Alena.Trojakova)

**CANARI** stands for **C**ode for the **A**nalysis **N**ecessary for **A**RPEGE for its **R**ejects and its **I**nitialization.
It is software (part of IFS/ARPEGE source code) to produce an ARPEGE/ALADIN analysis based on optimum interpolation method. The number of ARPEGE/ALADIN configuration is 701. CANARI has the two main components the quality control and an analysis. According to the type of observations the analysis can be:
* 3D multivariate for U, V, T, Ps
* 3D univariate for RH
* 2D univariate for 2m/10m fields
* soil parameters analysis is based on 2m increments

CANARI can handle following 10 types of observations:
* SYNOP: Ps, T2m, RH2m, 10m Wind, RR, Snow depth, SST
* AIREP: P ( or Z), Wind, T
* SATOB: P, Wind, T - from geostationary satellite imagery
* DRIBU: Ps, T2m, 10m Wind, SST
* TEMP: P, Wind, T, Q
* PILOT: Wind with the corresponding Z, (sometimes 10m Wind)
* SATEM: Q, T retrieved from radiances- surface


