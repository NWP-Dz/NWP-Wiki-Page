# Postprocessing with FULL-POS

## Introduction

FULL-POS is a powerful postprocessing package, which is part of the 
common ARPEGE/IFS cycle. FULL-POS is documented by 
 * [Yessad (2011)](http://www.cnrm.meteo.fr/gmapdoc/spip.php?article157): This documentation describes the software FULL-POS doing post-processing on different kind of vertical levels. In particular, post-processable variables and organigramme are given. Some aspects of horizontal and vertical interpolators (which may be used in some other applications) are also described.
 * [ykfpos38.pdf](http://www.cnrm.meteo.fr/gmapdoc/IMG/pdf/ykfpos38.pdf): FULL-POS in cycle 38
 * [El Khatib (2002)](http://www.cnrm.meteo.fr/gmapdoc/spip.php?article17): Older documentation with a link to an old FULL-POS website.

FULL-POS is a special configuration (9xx) of the full model for setup and initialization. In other words it is a 0 hour forecast, with extra namelist settings for variables to (post)process and to write out. When generating initial or boundary files we are calling a special configuration of FULL-POS, e927.
 

