# **Self-Driving Car Engineer - Nanodegree** 
## ---Computer Vision---
# Advanced Lane Line Detection

---
[//]: # (Image References)

[image1]: ./writeup/left.jpg "left image"
[image2]: ./writeup/center.jpg "center image"
[image3]: ./writeup/right.jpg "right image"
[image4]: ./writeup/woflip.jpg "without flip image"
[image5]: ./writeup/wflip.jpg "with flip image"
[image6]: ./writeup/cropped.jpg "cropped image"
[image7]: ./writeup/nvidia.jpg "NVDIA Architecture"
[image8]: ./writeup/loss.jpg "Output of loss metrics"


## Introduction
In this project the goal is to write a software pipeline to identify the lane boundaries in a video from a front-facing camera on a car. In project one we already implemented a simple lane line detection pipeline. Here we are using more complex computer vision technics for processing the images which leads to a better lane line detection than in project one.

The video shows the result of this project.


![result video](./writeup/run1.gif) 



The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Apply a perspective transform to the undistorted image ("birds-eye view").
* Use color transforms, gradients to create a thresholded binary image.
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.


My project includes the following files:
* model.py: containing the script to create and train the model

* README.md: summarizing the results


---
## Camera Calibration
When a camera looks at 3D objecst in the real wold and transforms them into 2D image, the transfomation causes distortion in the 2D image. The calibration of the camera produces the distortion coefficients which needed to undistort the images, which are taken with the camera. To correct the distortion a chessboard will be used because the undistorted dimensions are known. We can compare the images of a chessboard taken by the camera in all diffrent kind of angles and distances with the known dimension of a chessboard and calculate the distortion coefficents using the OpenCV functions findChessboardCorners() and calibrateCamera() and so undistort the images.

image camcali

## Distortion Correction
To apply the distortion to the raw images the distortion coefficients which are calculated in the camera calibration will be used. 
The image below shows three of the test images before and after the distortion.

IMAGES

These three images will be further used in this report to show all the steps which are taken to detect the lane lines. These images are choosen because they are good examples for demonstrating the typical difficulties while lane line detection like shadows and diffrent colours of the pavement. 

## Persective Transform
A perspective transform maps the points in a given image to different, desired, image points with a new perspective. Here we use the perspective transform in a bird’s-eye view that let’s us view a lane from above. This will be useful for calculating the lane curvature later on.
The figures below show the process steps of the perspective transformation.
First 
|original image|flipped image|
|:--------:|:------------:|
|![alt text][image4]| ![alt text][image5]| 


##  Detect Lane Pixels


## Curvature and Vehicle Position

## Warp back

## Visual Output


 ## Discussion
 
Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?

Discussion includes some consideration of problems/issues faced, what could be improved about their algorithm/pipeline, and what hypothetical cases would cause their pipeline to fail.
 
