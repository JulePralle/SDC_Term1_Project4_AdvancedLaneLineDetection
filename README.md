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
The goal of this project is to write a software pipeline to identify the lane boundaries in a video from a front-facing camera on a car. In project one we already implemented a simple lane line detection pipeline. Here we are using more complex computer vision technics for processing the images which leads to a better and more robust lane line detection.

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
When a camera looks at 3D objecst in the real wold and transforms them into 2D image, the transfomation causes distortion in the 2D image. To correct the distortion a chessboard will be used because the undistorted dimensions are known. We can compare the images of a chessboard taken by the camera in diffrent kind of angles and distances with the known dimension of a chessboard and calculate the distortion coefficents by using the OpenCV functions findChessboardCorners() and calibrateCamera() and in the end undistort the images.

image camcali

## Distortion Correction
To apply the distortion to the raw images the distortion coefficients which are calculated in the camera calibration will be used. 
The image below shows three of the test images before and after the distortion.

IMAGES

These three images will be further used in this report to show all the steps which are taken to detect the lane lines. These images are choosen because they are good examples for demonstrating the typical difficulties while lane line detection like shadows and diffrent colours of the pavement. 

## Persective Transform
A perspective transform maps the points in a given image to different, desired image points with a new perspective. Here we use the perspective transform in a bird’s-eye view that let’s us view a lane from above. This will be useful for calculating the lane curvature later on.

The figures below show the process steps of the perspective transformation. The red marks are showing the source points in the left image and the destination points in the the right and already warped image. 

First 
|original image|flipped image|
|:--------:|:------------:|
|![alt text][image4]| ![alt text][image5]| 


## Color transforms and thresholding
Lane lines sometimes are better to detect when the image is converted to another color space. The most common color spaces are grayscale, RGB and HSV. The next figures will show one image with difficult conditions for lane line finding (shadows and diffrent pavement colors) converted to diffrent color spaces. To take out the lane line information the color spaces will be thresholded so that they in the end give out a binary image. The thresholds were choosen by trail and error.

IMAGES

The images in the figure show that the S channel is doing a robust job of picking up the lines under very different color and contrast conditions, while the other selections look messy. 
HIER WEITER BESCHREIBEN
r auch ok --mit rein?
sonst mit h channel werden die schatteb gut ausgeglichen und als cimbi siehts gut aus

COMBI IMAGE 

## Gradient thresholding
The canny edge detections used in the first project of lane line detections finds all possible edges in an image which is a lot of information. To reduce the amount of infomation and still keep the important data imformation, the soble operator of the canny edge detection will be thresholded. The x sobel is doing in most conditions the better job. The have just one threshold for the x and y gradient, the magnitude of the gradients can be used. The magnitude combines the x and y sobels as an absolut value of gradient. The figure below shows the thresholded x and y sobel operator and the magnitude of the gradient.

BILD X Y SOBEL


In case of lane lines we are only interested in edges of a particular orientation. With calculating the direction of the gradient by taking the inverse tangent of the y gradient devided by the x gradient. To sort out the unimportant directions of the gradient the result will be thresholded. The next images shows the thresholded direction of the gradient.

BILD DIR

## Combination of Color and Gradient Thresholding
To find the best thresholding and combination trail and error was used. In the and the final binary is a combination out of:

HIER NOCHG EINTRAGEB
  *R
  * S
  * H
  * mag
  * dir

After all thresholding there is a final binary image which will be used to detect the lane lines in the furter steps.

##  Detect Lane Pixels
After applying calibration, thresholding, and a perspective transform to a road image, you should have a binary image where the lane lines stand out clearly. However,Now after applying calibration, thresholding, and a perspective transform to a road image  we have a binary image where the lane lines stand out clearly.  However, I still need to decide explicitly which pixels are part of the lines and which belong to the left line and which belong to the right line.

I first take a histogram along all the columns in the lower half of the image (figure below) to detect the peaks as starting points of the two lane lines.


HISTOGRAMM

With the starting points of the histogramm I implemented sliding windows and fit a polynomial to the lanes. This technique is done by searching for the nonzero pixels in every single sliding window area and center the window according to the found nonzero pixels. The image shows the result of the sliding winwow technique.

SLIDING WINDOW IMAGE

Having the information out of the sliding window calculation the searching window technique can be implemented
Now I know where the lines are I can implement the searching window technique fo the next frame of video so I don't need to do a blind search again, but instead I am going just search in a margin around the previous line position. The result of the searching window technique is shown in the image below.

SEARCHING WINDOW


## Curvature and Vehicle Position
Now I have a thresholded image, where I have estimated which pixels belong to the left and right lane lines , and I have fit a polynomial to those pixel positions. To compute the radius of curvature of the fit.
R
​curve
​​ =
​∣2A∣
​
​(1+(2Ay+B)
​2
​​ )
​3/2
​​ 
​​ 


## Warp back

## Visual Output


 ## Discussion
 
Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?

Discussion includes some consideration of problems/issues faced, what could be improved about their algorithm/pipeline, and what hypothetical cases would cause their pipeline to fail.
 
