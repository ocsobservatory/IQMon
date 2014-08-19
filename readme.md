# Readme: IQMon

Copyright © Dr. Josh Walawender (email: jmwalawender@gmail.com). All rights reserved.


## Overview

IQMon is a python module which can be used to quickly analyze an image for on the fly reports of image quality (Image Quality Monitor = IQMon).  It was originally written to provide a quick look analysis of data from robotic telescopes.

The base functionality is that it uses SExtractor to find stars in the image and report the typical Full Width at Half Max (FWHM) and ellipticity.  This allows quick and dirty evaluation of the image quality in near real time (a few to tens of seconds on modest hardware circa 2010).

If the image contains a WCS, the module can also compare the WCS coordinates of the central pixel to the pointing coordinates in the image header to determine the pointing error.  If no WCS is present, the module can also attempt to solve the astrometry in the image using the astrometry.net solver.  

In addition, the software can use SCAMP and SWarp to determine a plate solution which includes distortion parameters and compare the instrumental magnitudes determined by SExtractor to catalog magnitudes to determine the photometric zero point of the image.

## Requirements

* python2.7.X or python3.X
* astropy (<http://www.astropy.org>)
* pyephem (<http://rhodesmill.org/pyephem/>)
* numpy
* matplotlib
* subprocess
* SExtractor (<http://www.astromatic.net/software/sextractor>)
* SCAMP (<http://www.astromatic.net/software/scamp>)
* SWarp (<http://www.astromatic.net/software/swarp>)
* astrometry.net solver (<http://astrometry.net>)

## Version History

* **v1.1**
    * Added ability to solve astrometry using SCAMP and rectify the image using SWarp
    * Added ability to solve for Zero Point using the SCAMP-solved data
    * Renamed many methods to better follow PEP8 conventions
    * Refactored `make_JPEG` to use matplotlib and PIL instead of ImageMagick.  The previous method is available as `make_JPEG_ImageMagick`.
* **v1.0.6**
    * Tweaks related to marking up of jpeg files.  Better markers, user settable maker size, and marks are labeled.
* **v1.0.5**
    * Bug fixes related to marking up of jpeg files.
* **v1.0.4**
    * MakeJPEG now marks the brightest 5000 stars rather than the first 5000 in the table.  Also annotates image to let viewer know more stars were detected.
    * added option to HTML output to choose which columns are displayed
    * other minor bug fixes and tweaks
    * fixed bug where axes in crop were reversed
* **v1.0.3**
    * Fixed handling of spaces in filenames by repalcing spaces in input file name with underscores when making working file.
    * Fixed bug in MakeJPEG which hard coded a binning of 2x2.
    * Added flag to ignore missing end card in fits header after running astrometry.net.
* **v1.0.2**
    * Fixed bug in jpeg creation which would crash when many (>~5000) stars were to be marked.  Now marks first 5000 stars.
    * Fixed bug in color coding of HTML.
* **v1.0.1**
    * Added astropy units converter between pixels and arcseconds to handle internal conversion of pixel scale.
    * Fixed bug in jpeg creation.  Will now handle rotation and marked stars.
    * Fixed bug when reading in summary text file.
* **v1.0** (released on github.com 2013/08/14)
    * Rewritten as object oriented code.  Implements most capabilities of v0.X.
    * Runs roughly 2x faster than v0.X.
* **v0.X** (frozen 2013/07/15)
    * Initial version, not under version control.
    * Deployed on live VYSOS data.
    * Analyzes image with SExtractor
        * Reports FWHM, ellipticity, background, number of stars detected
    * Reports image pointing info (alt, az, moon angle)
    * Can solve image with astrometry.net (with poor error handling)
    * Reports pointing error and position angle
    * Makes jpegs of image and cropped version with circles overlayed on stars found by SExtractor
    * Makes HTML and text file versions of results with one line per image, usually one night of images per file.


## Code Structure

IQMon functionality centers around the use of two objects:  IQMon.Telescope and IQMon.Image.  When used, you must create one of each of these objects.

* IQMon.Telescope object and holds detailed information on the telescope used to take the data and some custom configuration parameters.

* IQMon.Image object holds information about the image and contains the methods which do all of the image analysis which then fills in the object properties.

### Example Use

Please see example_MeasureImage.py file.

## License Terms

Please see LICENSE file.
