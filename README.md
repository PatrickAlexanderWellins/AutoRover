# Rover Project

Objective - Program an autonomous rover to map terrain and pick up rock samples.

## Functions for identifying rock samples and obstacle terrain

**Rock Identification**

For rock identification and mapping, I used a function called 'find_rocks'. The 'find_rocks' function takes an image and a tuple called 'levels' as inputs. The objective of this function is to take the input image that contains a rock and apply color manipulations in a manner that isolate the rock in the image. The meaning of 'isolate' in this context means the rock shows up as white in the image, and everything that is not a rock in the image shows up as black. The tuple 'levels' contains three numerical values that determine which pixel values are True or False in the three color channels of the input image. These boolean values are stored in a variable named rockpix at specific indices. A variable named 'color_select' is initialized with zeros in with the same width and height dimensions as the rock image, but only one layer deep. The indices that have the value 'True' in rockpix set the corresponding indices 'color_select' to the number 1, all other values in 'color_select' remain with the value 0. The 'color_select' variable is the output of the 'find_rocks' function, this output image either contains an image of a white rock with a black background if a rock is present in the image, or a black background if there is no rock present in the image. The image below contains an image of a rock that is an output of the 'find_rocks' function.

![](IsolatedRock.png)

**Obstacle Idendtification**

Obstacles are designated as terrain that is not navigable in the simulation environment. The images that are recorded while the rover is driving are converted to a 'warped' image. The warped image is a top down view of the environment that is in front of the rover. The warped image goes through a process where the pixel values are manipulated in a manner that produces a black and white version of the warped image. The white pixels in this new image represent the navigable terrain in the map, the black pixels represent the 'obstacle' terrain. To accurately map the obstacle terrain, all of the pixel values in the black and white warped image have the 1 subtracted. This creates an image that is represented by 0's and - 1's, the absolute value is taken to convert the - 1's to 1's. This process of identifying obstacles in the environment is the reverse of identifying navigable terrain. Identifying obstacles in the environment is a requirement to accurated creating a map of the environment. 

## Process Image Function

The process image function contains functions and variables that create the map of the environment. To create the map of the environment, the original Rover camera images have to be processed to the point where they represent information about navigable terrain, rock samples, and obstacles in the same coordinate system as the worldmap. The first step is to take the original image and convert it to a top down view, so the original image looks like a 2-D image, this image is called warped. A color threshold is applied to the warped image, this color threshold makes navigable terrain is show up as white. This image is then converted into Rover coordinates, and the Rover coordinates are then converted to world coordinates. The conversion from one coordinate system to another is done by rotatating and translating pixels. 

In addition to processing the images from the Rover to world coordinates. The pixels that represent navigable terrain are plotted plotted blue in the world, obstables are plotted red and rocks are plotted white on the world map.

## Perception and Decision step

**Perception**

The 'perception_step' function takes as input an instance of a Rover object. The Rover object has a current image which is processed to create a map of navigable terrain, obstacle terrain, and rocks in the environment. This is achieved by using many of the functions and variables that are present in the 'process_image' function. The 'perspect_transform' function converts the rover camera image into a top down (warped) image, this image is processed with the 'color_threshold' function. Pixel values are manipulated to isolate navigable terrain, obstacles and rocks. The images that contain navigable terrain, obstacles, and rocks are then converted to rover coordinates which are then converted to world coordinates. Translation to world coordinated is necessary for creating a map of the environment. The pixels that represent navigable terrain increase the blue channel of the Rover.worldmap by 10, and the obstacle pixels increase the Rover.worldmap red channel pixels by 1. If there is a rock in the environment, the Rover.worlmap images green channel is set to 255 where there are rocks.

To increase the fidelity of the map, I included an if statement that requires the roll of the rover to be in a specific range for the mapping to take place. In addition to mapping the environment, information from the image is used in the decision function for controlling the rovers actions. The 'Rover.nav_angles' variable is used in the decision step to control the steering angle of the rover. The amount of navigable space in front of the rover is additional information that is gained from the perception step. The navigable space in front of the rover determines the course of action for the rover.

**Decision**

The decision_step function takes an instance of a Rover object as input and returns a Rover object as output. The decision_step function changes the rovers throttle, steering angle, and brake. These variables are dependent on the terrain that surrounds the rover. The condition for the rover to move forward depends on there being sufficient navigable terrain in front of the rover. The steering angle that the rover takes when the rover has sufficient navigable terrain in front of it depends on the 'Rover.nav_angles' variable. The steering angle is shifted to the direction of the navigable terrain in front of the rover, the steering angle is clipped by +/- 15 degrees. When there is insufficient terrain in front of the rover, the rover stops and turns until there is sufficient navigable terrain on front of the rover. When there is sufficient terrain in front of the rover the rover begins to move forward again, and the steering resumes in the fashion that was previously described. The change in the variables, throttle, steer and brake is the actuation of the rover. The fundamental processes of robotics includes perception, decision making, and actuation. The perception and decision functions control the fundamental processes of the rover.

