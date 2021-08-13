## 3. Features

* user can define a grid in geographic Cartesian (northing/easting) coordinates provided in the form of a Digital Elevation Model (DEM) with arbitrary EPSG projection code (but has to be in northing/easting; lat/lon won't work), 
* the program will extract the portion of the grid that overlaps with the given co-registered image pair, 
* for radar-coordinate imagery, use radar orbit information plus DEM along with GDAL coordinate transformation to precisely map the geolocation and the motion velocity at each grid point (in geographic Cartesian coordinates) to the corresponding pixel index and pixel displacement (in imaging coordinates), where the imaging along-track and line-of-sight unit vectors are precisely derived at each grid point
* for Cartesian-coordinate imagery, use map coordinate information of the image pair along with GDAL coordinate transformation to precisely map the geolocation and the motion velocity at each grid point (in geographic Cartesian coordinates) to the corresponding pixel index and pixel displacement (in imaging coordinates), where the imaging horizontal- and vertical-direction unit vectors are precisely derived at each grid point
* the geographic z-direction motion velocity is estimated using the irrotational flow assumption as well as inputs from the geographic x- (easting) and y- (northing) direction motion velocity maps and the geographic x- (easting) and y- (northing) direction local surface slope maps
* return the pixel indices in the image pair for each grid point
* return the pixel displacement given the motion velocity maps and the local surface slope maps in the direction of both geographic x- (easting) and y- (northing) coordinates (they must be provided at the same grid as the DEM)
* return the matrix of conversion coefficients that can convert the fine pixel displacement between the two images (estimated with the autoRIFT Python module https://github.com/leiyangleon/autoRIFT) to motion velocity in geographic x- (easting) and y- (northing) coordinates
* the current version can be installed with the ISCE software (that supports both radar and Cartesian coordinates) or as a standalone Python module (Cartesian coordinates only)
* when used in combination with the autoRIFT Python module (https://github.com/leiyangleon/autoRIFT), Geogrid can be used for feature tracking between image pair over a grid defined in an arbitrary geographic Cartesian (northing/easting) coordinate projection
* outputs are returned in geocoded GeoTIFF image file format with the same EPSG projection code as input search grid
* spatially varying input maps of velocity search range (in units of m/yr), chip size minimum and maximum (in units of m), stable surface mask (boolean) can be handled, with corresponding output (in units of integer image pixels) returned at each grid point.
* **[NEW]** For feature tracking of optical images, the program now supports fetching optical images (Landsat-8 GeoTIFF and Sentinel-2 COG formats are included) as well as other inputs (e.g. DEM, slope, etc; all in GeoTIFF format) from either local machine or or remotely using [GDAL virtual file systems](https://gdal.org/user/virtual_file_systems.html) (e.g., `/vsicurl/https://...`). See the changes on the autoRIFT [commands](https://github.com/leiyangleon/autoRIFT). Geogrid will now always perform coregistration. For feature tracking of radar images, the program also supports fetching auxilliary inputs (e.g. DEM, slope, etc; all in GeoTIFF format) from either local machine or remotely.