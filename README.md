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

#### Heading 4
##### Heading 5
###### Heading 6
