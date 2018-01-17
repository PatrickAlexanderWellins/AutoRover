# Rover Project

Objective - Program an autonomous rover to map terrain and pick up rock samples.

## Functions for identifying rock samples and obstacle terrain

**Rock Identification**

For rock identification and mapping, I used a function called 'find_rocks'. The 'find_rocks' function takes an image and a tuple called 'levels' as inputs. The objective of this function is to take the input image that contains a rock and apply color manipulations in a manner that isolate the rock in the image. The meaning of 'isolate' in this context means the rock shows up as white in the image, and everything that is not a rock in the image shows up as black. The tuple 'levels' contains three numerical values that determine which pixel values are True or False in the three color channels of the input image. These boolean values are stored in a variable named rockpix at specific indices. A variable named 'color_select' is initialized with zeros in with the same width and height dimensions as the rock image, but only one layer deep. The indices that have the value 'True' in rockpix set the corresponding indices 'color_select' to the number 1, all other values in 'color_select' remain with the value 0. The 'color_select' variable is the output of the 'find_rocks' function, this output image either contains an image of a white rock with a black background if a rock is present in the image, or a black background if there is no rock present in the image. The image below contains an image of a rock that is an output of the 'find_rocks' function.

![](IsolatedRock.png)

**Obstacle Idendtification**



A variable called 'obs_map' converts the the color thresholded image into an image that represents the obstacles on the map. This is done by taking the color thresholded image that originally shows navigable terrain, subtracting one from the navigable terrain pixel values, then taking the absolute value of the of the new image. This has the effect of converting what is not navigable terrain into an obstacle. The obstacle image then goes through a final step of processing where the mask image clips the obstacle image. This has the effect of preventing what is outside the view of the Rover from being considered an obstacle.

## Process Image Function

The process image function contains functions and variables that create the map of the environment. To create the map of the environment, the original Rover camera images have to be processed to the point where they represent information about navigable terrain, rock samples, and obstacles in the same coordinate system as the worldmap. The first step is to take the original image and convert it to a top down view, so the original image looks like a 2-D image, this image is called warped. A color threshold is applied to the warped image, this color threshold makes navigable terrain is show up as white. This image is then converted into Rover coordinates, and the Rover coordinates are then converted to world coordinates. The conversion from one coordinate system to another is done by rotatating and translating pixels. 

In addition to processing the images from the Rover to world coordinates. The pixels that represent navigable terrain are plotted plotted blue in the world, obstables are plotted red and rocks are plotted white on the world map.

## Perception and Decision step

**Perception**

The perception_step function takes as input an instance of the Rover object. The Rover object has an associated image which is processed to create a map of navigable terrain, obstacle terrain and rocks in the environment. This is achieved by using many of the functions and variables that are present in the process image function. The perspect transform converts the current rover camera image into a top down image, this image is processed with the color threshold function. The image is then converted to rover coordinates and then converted to world coordinates. The pixels that represent navigable terrain increase the blue channel of the rover world map by 10, and the obstacle pixels increase the world maps red channel pixels by 1. To increase the fidelity of the map, I included an if statement that requires the roll of the rover to be in a specific range for the mapping to take place. There is also a conditional statement that checks if there is a rock present in the rover's view. If there is a rock present, the rock is plotted on the world map. The Rover.nav_angles variable is also set in the perception_step function. The Rover.nav_angles variable is used in the decision step to control the steering angle of the rover.

**Decision**

The decision_step function takes an instance of a Rover object as input and returns a Rover object as output. The decision_step function changes the rovers throttle, steering angle, and brake. These variables are dependent on the terrain that surrounds the rover. The condition for the rover to move forward depends on there being sufficient navigable terrain in front of the rover. The steering angle that the rover takes when the rover has sufficient navigable terrain in front of it depends on the Rover.nav_angles variable. The steering angle is shifted to the direction of the navigable terrain in front of the rover, the steering angle is clipped by +- 15 degrees. When there is insufficient terrain in front of the rover, the rover stops and turns until there is sufficient navigable terrain on front of the rover. When there is sufficient terrain in front of the rover the rover begins to move forward again, the steering resumes in the fashion that was previously described.

