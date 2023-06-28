# Land Use in Eastern Iberia
(c) 2023. C Michael Barton, Arizona State University. See CITATION.cff for full citation of this dataset.

Data and R markdown script supporting analyses described in the paper:

Cegielski, W.H., Snitker, G., Barton, C.M., Cortell Nicolau, A., Pardo Gordò, S., Bergin, S.M., Diez Castillo, A., 2023. A multi-method approach with machine learning to evaluating the distribution and intensity of prehistoric land use in eastern Iberia. _Quaternary International_ In press.

## Data files include:
(All data in Unicode UTF-8 character set)

* __medland_survey2014_2017.csv:__ lithic surface collections from surveys in 3 study areas.
* __training_data_E_Iberia.csv:__ lithic assemblages from excavation, dated contexts in eastern Iberia. These data were used for training the Random Forest model used for chronological unmixing.
* __C14_S&E_Iberia_all.csv:__ radiocarbon dates from eastern and southern Iberia used for SPD analysis.

### Metadata for Data Files
(See paper for more detailed information)

__study.area:__ valley surveyed  
__zone:__ sampling stratum  
__sector:__ survey block  
__subsector:__ survey patch/collection unit  
__visibility:__ visibility of ground surface  
__collection:__ total number of artifacts collected  
__time.date:__ time and date of survey  
__xcoord:__ survey patch centerpoint coordinate  
__ycoord:__ survey patch centerpoint coordinate  
__area.sqm:__ area of survey patch  
__Counts of each lithic type collected in survey patch__  

__Description of Lithic Types__  

| Type Code	| Type Name	| Description |
| ---	| ---	| --- |
| undiag.lithics	| undiagnostic lithics	| undifferentiated lithics, including:  unretouched flakes, chunks/debris, irregularly or minimally retouched flakes, and unprepared flake cores
| flake.core	| flake cores	| prepared flake cores (e.g., levallois and discoidal cores) |
| MP.tools	| Middle Paleolithic retouched artifacts	| Large bifaces ("handaxes"), Mousterian points, and side scrapers (all forms) |
| notch.dent	| notches & denticulates	| single notches & series of notches (denticulates) |
| blade.tech	| prismatic blade/bladelet technology	| unretouched blades/bladelets, prismatic blade cores, & core preparation/rejuvenation flakes |
| burins	| burins	| burins (all forms) |
| end.scraper	| end scrapers	| end scrapers (all forms) |
| ret.blade	| retouched blades	| marginally retouched blades |
| invret.blade	| invasively retouched blades	| invasively retouched blades |
| microburin	| microburins	| microburins |
| backed	| backed pieces	| backed bladelets, backed points, and backed small flakes |
| trapeze	| trapezes	| trapeze microliths |
| triangle	| triangles	| triangle microliths |
| truncation	| truncations	| truncated blades and bladelets |
| bifacial.pt	| bifacial points	| bifacial projectile points (all forms) |
| dent.sickle	| denticulated sickle blades	| denticulated sickle blades (usually with silica sheen) |

## GIS Files of Survey, Sampling Protocol, and Interpolation for Occupational Ubiquity and Land Use Intensity
In ESRI shapefile format. Polygons of all surveyed patches in 3 valleys in eastern Spain. See paper for more information.

For each of the 3 study areas (Canal de Navarrés, Hoya de Buñol, & Cocina-Catadau), there are shapefiles for each study area, zones (sampling strata), and subsectors (survey patches). For Navarrés and Buñol there are also files for sectors (survey blocks).

### Metadata for GIS Files
__study.area:__ valley surveyed
__zone:__ sampling stratum
__sector:__ survey block
__subsector:__ survey patch/collection unit

### Spatial Interpolation for Occupational Ubiquity and Land Use Intensity
Chronological unmixing created probability values for ubiquity and intensity for each survey patch with artifacts, linked with the coordinates of the center point of each survey patch (see published paper and R scripts included here). Points generated from the center point coordinates were used to create raster maps of ubiquity and intensity using bilinear spline interpolation for each time period within each study area. The interpolation was carried out in GRASS GIS version 7.4.4 with the _v.surf.bspline_ module. This can be done in the GRASS GUI or on the command line. The relevant command is:

__v.surf.bspline input=_[points file]_ column=_[probabilities]_ raster_output=_[interpolated raster]_ ew_step=300 ns_step=300__

where...
* _[points file]_ is the name of the GIS file of survey patch center points that also has ubiquity and intensity probabilities for each time period.
* _[probabilities]_ is the column of probability values to interpolate (i.e., ubiquity or intensity for each time period).
* _[interpolated raster]_ is the name of the raster file created through interpolation.

After interpolation, each raster was filtered to remove any negative probability values generate by the spline algorithm using the following GRASS raster map calculator command.

__r.mapcalc expression="_[filtered raster]_ = ( ( _[interpolated raster]_ >= 0 ) * _[interpolated raster]_ )"__

## R scripts
* __land-use_in_E-Iberia.Rmd:__ R-Markdown script to carry out visibility analyses, Random Forest analyses to calculate age estimates of assemblages, and SPD (summed probability distribution) analysis of radiocarbon dates from eastern and southern Iberia
* __land-use_in_E-Iberia.nb.html:__ HTML output file of analyses carried out with R-Markdown script
