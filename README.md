# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

To complete the project, two files will be submitted: a file containing project code and a file containing a brief write up explaining your solution. We have included template files to be used both for the [code](https://github.com/udacity/CarND-LaneLines-P1/blob/master/P1.ipynb) and the [writeup](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md).The code file is called P1.ipynb and the writeup template is writeup_template.md 

To meet specifications in the project, take a look at the requirements in the [project rubric](https://review.udacity.com/#!/rubrics/322/view)


# Finding Lane Lines on the Road - Kiran Puttur
----


#### The goal of this project is to identify lane lines on the road and draw them on the test images and videos. 


# Reflection
---

## 1. The Pipeline

#### The Pipeline consists of:
* [Choosing the Color Model - RGB, HSV and HSL]
* [ Grayscaling and smoothing using Gaussian Blur]
* [Canny Edge Detection]
* [Selecting Region of Interest]
* [Hough Transform]
* [Embedding Lines on Image]

Last but not least
* [Extending the draw_lines function]

## Choosing the Proper Color Space
---
There are different color models available such as

* RGB
* HSV ( Hue Saturation Value)
* HSL (Hue Saturation Lightness)

More about HSV and HSL here : https://en.wikipedia.org/wiki/HSL_and_HSV if we use the RGB model then the shadows from objects like trees and clouds will be much more darker, to avoid this we can either use HSL or HSV color models.

## Grayscaling and Smoothing using Gaussian Blur

In this section we will grayscale the after applying the HSL filter and then use Gaussian blur for smoothing the edges before presenting to Canny Edge Detection , kernel size of 9 is used here.


### Canny Edge Detection

Canny Edge Detection
Canny Edge detection is an algorithm see here to detect a wide range of edges in images. It was developed by John F. Canny in 1986. 
Canny also produced a computational theory of edge detection explaining why the technique works. It goes via multistage starting with 
Noise reduction using Gaussian filter, finding intensity gradient of image with Sobel kernel both in horizontal and vertical direction.
 In this function we will set high_threshold and low_threshold so narrow done the boundaries for an acceptable edge/value.


### Selecting Region of Interest (ROI)
After **selecting the colors, gray scaling, smoothing, and getting the image's Canny edges** 
the images still have white spots around the other unimportant parts of the image like trees, sky, clouds etc.
We need to mask this out and select only the parts which are of interest to us, so to choose region of interest 
we can make use of the helper function region_of_interest. The method takes two argument image and vertices that 
make up a polygon, it sets any pixel outside the polygon to black and leaves the pixels within the polygon as it is.

### Hough Transform
In Hough space, I can represent my "x vs. y" line as a point in "m vs. b" instead. 
The Hough Transform is just the conversion from image space to Hough space. 
So, the characterization of a line in image space will be a single point at the position (m, b) in Hough space. 
Each point in image space corresponds to a sine wave in Hough Space and if some points in image space lay on the 
same line in Image space this corresponds to an Intersection(Hough Point) of the points sine waves. 
Provided hough_lines method takes in the parameter threahold that is the minimum number of intersections(votes) in Hough Space to constitute a line in image space. More information here: https://en.wikipedia.org/wiki/Hough_transform


### Draw Lines on Image

Draw lines on Image just iterates over the images and embeddes the lane lines drawn using apply_hough_lines on top of the existing
image.


#### Improving the draw_lines function.

To extend the draw_lines method or to extrapolate we need to find out the two lane lines(slope, y-intercept) by dividing our lane lines as left and right using their slopes negative slope indicates left lane and positive means right lane, then we need to average each line slopes and y-intercepts to get a single line for each lane.

once we got the averaged slope and y-intercept for each lane line, then according to forumla y = mx + b, we know m which is slope and b is the y-intercept.

Only thing to left to find out is the x value which can be derived as x = (y - b)/m

This is largely from the discussions here : https://discussions.udacity.com/t/error-using-the-provided-hough-lines-function/213164 and in #slack channel https://discussions.udacity.com/t/error-draw-lines-function/232114/30


## 2. Potential shortcomings with the pipeline

Here are so shortcoming that I need to address in the coming weeks as I advance through the course:

1. Identifying curves and extending, improving the draw_lines function to match curves
2. Ability to improve the process image function for different frame rates/
3. More robust and dynamic way of identifying the road's horizon
4. Various scenarios such as white cars crossing in front of us or lane mapping in a jug shaped exit ramps, or lane mapping in entry ramps.
5. Willt this work under low light or rainy situations where visibility is less.

## 3. Possible improvements could be

1. Improving the readability of the code, constructing a class and neatly tucking methods underneath it.
2. Ability to improve lane mapping using high speed camera inputs, frames more than 60 fps.
2. Ability to map curves on the way
