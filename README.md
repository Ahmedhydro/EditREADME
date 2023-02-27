# Flood Inundation Mapping Using Python
  This code has meant to automate the process illustrating the flood affected areas in past flooding events using Sentinel 2 images.This tool can help to evaluate what areas are prone to flood and how are those areas classified.
  
![sentinel_2](https://user-images.githubusercontent.com/118891377/221658904-5cfc75a9-9c82-4f5f-a4d6-b5d37e2f2727.jpg)
  
  Sentinel_2 Satelite (source:https://eox.at/2015/12/understanding-sentinel-2-satellite-data/)
## Goals:
* This Project aims to calculate flood extent using the Normalized Difference Water Index (NDWI).
* And then categorize the inondated areas utilizing Building Index, Vegetation Index and Bare Index (NDBI, NDVI and BSI) respectively. 

## Usage/Run instructions:
* The code asking for input of 2 images, first a baseline image .SAFE folder  and the a flood image .SAFE folder.
* The code also asks if you want to plot the results or not.
* The outputs are saved in the Data/Output folder.

## Requirements:
* GDAL Library
* Numpy Library
* OS Library
* xml .etree .ElementTree Library
* Matplotlib Library
* Logging Library

## Available Data and Code Structure
The following flow chart illustrates the provided code and data. Functions, methods, and files have been excuted in this project.

![uml](https://user-images.githubusercontent.com/118891377/221654258-301c0828-e8aa-498f-bd79-e24be442f5af.jpg)

Figure (1): UML Diagram

## Code
### config.py
In this project, the main required libraries (gdal, os and numpy) have been preserved in config.py file besides the logging module to identify the prospcted errors
  
  ### Sentinel.py (class)
  This class created to handle the satelite image data to be able used for imagery processing multi purposes.Most important informations hve to be defined in the sentinel_2 data folder are :
  * The path to the folder containing the Sentinel-2 data (folder_path) (string)
  * The resolution of the satellite image in meters (integer)
  * The True Image Colour dataset (TCI) (gdal.dataset)
  * The spatial information (geoLoc) for TCI dataset (tuple)
  * The Projection of the TCI dataset (string)
  
  Many basic functions have been used in Sentinel.py
  #### get_array
  Returns an array of the specified band number with NaN values for cloud data. It returns a numpy array from the selected band of Sentinel 2A Level 2 processed data based on the required band number(band_num) of sentinel 2A for a certain process in form of 3 characters.
   
 #### remove_cloud_data
  Replaces cloud data in the given raster with NaN values. This function removes the pixels classified as High probability cloud, Medium Probability cloud and cloud shadow from the given sentinel 2 raster in each raster array (raster_array)
        
  #### get_band
  Returns the path to the file of the specified band number. It Returns a path to the band number selected as band_num
  
            
### func.py        
 This class contains  xml. etree.ElementTree libarary as ET and several functions to obtain the sentinel data information which enable the code to create a new raster based on the desired image procesing 
 
#### read_safe_metadata
 This function returns image acquisition parameters of the Sentinel image(image_type) (string) whose path is provided as safe_folder (string).These information will into a certain file (file_name)

#### check_path_validity
This function checks the validity of a path and also has the functionality to check if the path ends with a certain extension(default value set to "None"). After checkin the path, we will get a confirmation about the validity of path (Returns a boolean)

#### get_mask_array
This function gets a mask array with values numpy.nan or 1 based on the set threshold. The function sets the values above the threshold to be 1 and below to be numpy.nan

#### area_from_mask
This function calculates the area of an array based on its pixel x and y size defined in gdal Geotransform parameter
* array: numpy array imported to the function on which area calculation is to be performed
* geo_loc: gdal GeoTransform object type imported for individual pixel size calculation
* threshold: threshold of values above which the pixel will be count for area calculation
* return:returns an integer value

#### set_nan
Replaces a specific value in the array with NaN.
* array: array imported into function
* value: value to be replaced by numpy.NaN
*return: returns an array

#### write_raster
Writes a NumPy array to a raster file using GDAL.
* array: numpy array imported to the function
* geo_transform: Gdal GetGeoTransform object for the array's geolocation
* projection: Gdal GetProjection object for the array's projection system
* output_path: path to the output folder
* Define the driver to use for the output raster
* Create a new raster with the given dimensions and data type
* Set the geo transformation and projection of the raster
* Write the array to the raster band
    
    
### flood.py (class)
This class represents the flood case which its data obtained from sentinel image captured during extreme flood event. This part of code aims to determine the inundated areas under the extreme flood using ndwi_array function

#### ndwi_array
This property returns the normalized difference water index (NDWI) array for the image. NDWI is computed as (green - NIR) / (green + NIR), where green is the green band and NIR is the near-infrared band.


### Baseline.py (class)
This class represents the baseline case which its data obtained from sentinel image captured during flood event which not exceed the average flood recordes.this class contains several function used to categorize the inundated areas.

#### ndwi_array
This property returns the normalized difference water index (NDWI) array for the image. NDWI is computed as (green - NIR) / (green + NIR), where green is the green band and NIR is the near-infrared band.

#### ndvi_array
This property returns the normalized difference vegetation index (NDVI) array for the image. NDVI is computed as (NIR - Red) / (NIR + Red), where green is the Red band and NIR is the near-infrared band.

#### ndbi_array
 This property returns the normalized difference building index (NDBI) array for the image. NDBI is computed as (SWIR1- NIR) / (SWIR1 + NIR), where SWIR1 is the Short Wave Infrared Band 1 and NIR is the near-infrared band.
 
 #### bare_soil_index_array
 This property returns the Bare soil index (BSI) array for the image. BSI is computed as (SWIR1 + Red - NIR - Blue) / (SWIR1 + Red + NIR + Blue), where SWIR1 is the Short Wave Infrared Band 1, Red is the Red band, Blue is the Blue band and NIR is the near-infrared band.

#### save_rasters
 This function saves the indices as rasters to a desired folder in .tif format.
 * output_path: Desired path where rasters want to be saved
 * ndwi: Boolean with default value set to "False", for saving the NDWI raster
 * ndvi: Boolean with default value set to "False", for saving the NDVI raster
 * ndbi: Boolean with default value set to "False", for saving the NDBI raster
 * bare_soil_index: Boolean with default value set to "False", for saving the BSI raster
 * return: If no rasters are selected, print a message (" No raster selected to be written, " "Use arguments 'ndwi=True' 'ndvi=True' and/or 'ndbi=True' to save rasters")

### plot.py (plot_flood_classification)
This function contains matplotlib.pyplot and numpy libraries to plot maps contain all the categories of the areas under the water in different colours

## Main.py (function) 
The main function when called, performs indices calculations using a user provided baseline image and an image acquired during flood from Sentinel 2 Level 2A Images from Copernicus Open Access Hub (https://scihub.copernicus.eu/dhus/#/home) The results saved from this program by default save in an Output folder within the directory in which the program is located.
* base_image: Folder path to (sentinel2image).SAFE folder for an image used for baseline indices analysis
* flood_image: Folder path to ( sentinel2image).SAFE folder for an image flood/water extent calculation
* resolution: Resolution of imagery to be used
* plot: Boolean for prompting for creating a plot from the resultant masks, default value set to "False"
* Assign classes to the image paths
* Read and print metadata
* Process flood image

##### PROCESS BASE IMAGE
* Calculate NDVI and create its flooded mask
* Calculate NDBI and its flooded mask
* Calculate Bare_Soil_Index and its flooded mask
* Save rasters to output folder
* Save flood mask
* Save NDBI, NDVI, Bare_Soil Index from the baseline image
* Save flooded area classifications
* Plot image



 
   
 
  

