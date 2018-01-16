# Rover Project
Build an autonomous rover to map terrain and pick up rock samples

## Functions for identifying rock samples and obstacle terrain

**Rock Identification**

To find rocks I used a function called find_rocks that takes an image as input and a tuple called levels. The objective of this function is to take in a image that contains a rock and output an image that only contains the rock. This is done by turning specific pixel values in the RGB color channels to True or False and then converting these boolean values to zeros and ones to make a black and white image. A variable named color_select is initialized with zeros in with the same width and height dimensions as the rock image, but without the RGB layers. Indices that are True in rockpix convert the same indices in color_select to take on the value 1. 

**Obstacle Idendtification**

A variable called obs_map converts the the color thresholded image into an image that represents the obstacles on the map. This is done by taking the color thresholded image that originally shows navigable terrain, subtracting one from the navigable terrain pixel values, then taking the absolute value of the of the new image. This has the effect of converting what is not navigable terrain into an obstacle. The obstacle image then goes through a final step of processing where the mask image clips the obstacle image. This has the effect of preventing what is outside the view of the Rover from being considered an obstacle.

## Process Image Function

The process image function contains functions and variables that create the map of the environment. To create the map of the environment, the original Rover camera images have to be processed to the point where they represent information about navigable terrain, rock samples, and obstacles in the same coordinate system as the worldmap. The first step is to take the original image and convert it to a top down view, so the original image looks like a 2-D image, this image is called warped. A color threshold is applied to the warped image, this color threshold makes navigable terrain is show up as white. This image is then converted into Rover coordinates, and the Rover coordinates are then converted to world coordinates. The conversion from one coordinate system to another is done by rotatating and translating pixels. 
