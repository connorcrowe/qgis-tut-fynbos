# QGIS Practice: Fynbos Habitat Locations
This is a part of a series of lessons completed by following the tutorials outlined in the [QGIS Training Manual](https://docs.qgis.org/3.34/en/docs/training_manual/index.html), which is a component of the QGIS Documentation. The purpose was to learn and practice QGIS and geospatial skills. The lesson and data used in the practice is not my own, but was provided either by the QGIS Training Manual or Open Street Map.
**Specific Skills:**
- Raster analysis
- Vector analysis
- 

## Problem
This analysis is looking to find areas near the University of Cape Town, South Africa, that are suitable habitats for a specific fynbos plant species. The criteria for the habitat are:
1. It grows on East facing aspects 
2. It grows on slopes with a gradient between 15% and 60%
3. It grows in areas that have total annual rainfall of > 1000 mm
4. It will only be found at least 250m away from human settlement
5. The area of vegetation in which it occurs should be at least 6000 m squared in area

### Data Sources
The QGIS Training Manual provided a zoning map of the region, a digital elevation model (DEM) and a map of rainfall. The first part of the solution involved importing these layers, projecting them into the correct CRS, clipping and filtering them.

## Solution
### Slope, Aspect and Rainfall
The raster analysis toolbox was used on the digital elevation model to create a hillshade layer with slope, and an aspect layer. The rainfall layer provided was filtered to only include areas with rainfall values above 1000mm. The slope layer was filtered to only include gradients between 15% and 60%, and the aspect layer was filtered to easterly values (between 45 and 135 degrees). 

These were combined in the raster calulator to create a layer representing only areas that met the slope, aspect and rainfall criteria (Criteria 1-3). This was vectorized and fixed to make the data usable in conjuction with the zoning and area analysis.

<span class="caption">Image 1: Slope map - Image 2: Aspect map - Image 3: Suitable slope, aspect and rainfall</span>
![](/images/slope.png) ![](/images/aspect.png) ![](/images/slope_aspect_rainfall.png)

### Zoning and Area
Next, the zoning layer was queried to only include rural zoning. A negative buffer was created to exclude areas within 250m of human settlement (Criteria 4).

Next the vector intersection tool found areas that were in both the aspect/slope/rainfall layer and the buffered rural zoning layer. An `$area` expression field was then added to the resulting layer so that it could be filtered done to only include areas larger than 6000m (Criteria 5).

Then, after digitizing the location of the University of Cape Town, the `join attributes by nearest` tool was used to determine which 4 of the suitable areas were closest to the university. 

![](/images/rural_w_buffer.png)

## Final Result
The final result is the following map of suitable areas, complete with the 4 closest areas to the unversity highlighted. 
![](/images/final.png)