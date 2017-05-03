# **Self-Driving Car Engineer - Nanodegree** 
## ---Computer Vision---
# Advanced Lane Line Detection

---
[//]: # (Image References)

[image1]: ./output_images/chess_cornerDraw_undis_12.jpg "Camera Calibration Chessboard"
[image2]: ./output_images/Undistortion_3.jpg "Undistortion of Image 3"
[image3]: ./output_images/Undistortion_5.jpg "Undistortion of Image 5"
[image4]: ./output_images/warp_7.jpg "Perspective Transform of Image 7"
[image5]: ./output_images/warp_2.jpg "Perspective Transform of Image 2"
[image6]: ./output_images/gray_and_thresh.jpg "Gray Image and Binary"
[image7]: ./output_images/RGB_and_thresh.jpg "RGB Channel and Binary"
[image8]: ./output_images/HLS_and_thresh.jpg "HLS Channel and Binary"
[image9]: ./output_images/color_stack_thresh.jpg "Combined Color Images and Binary"
[image10]: ./output_images/sobels_and_magnitude.jpg "Sobelx, Sobely and their magnitude"
[image11]: ./output_images/direction.jpg "Direction of the Gradient"
[image12]: ./output_images/allcombined.jpg "Combined Color and Gradient Thresholding"
[image13]: ./output_images/histogram.jpg "Histogram of Pixels"
[image14]: ./output_images/SlidingWindows.jpg "Sliding Windows"
[image15]: ./output_images/SearchWindow.jpg "Searching Windows"
[image16]: ./output_images/OriginalwithLaneArea.jpg "Warp back onto original image"
[image17]: ./output_images/ResultImageText_3.jpg "Result of Image 3 with Text"
[image18]: ./output_images/ResultImageText_5.jpg "Result of Image 5 with Text"
[image19]: ./output_images/Summary_1.jpg "Summary of Image 1"
[image20]: ./output_images/Summary_2.jpg "Summary of Image 2"
[image21]: ./output_images/Summary_3.jpg "Summary of Image 3"
[image22]: ./output_images/Summary_4.jpg "Summary of Image 4"
[image23]: ./output_images/Summary_5.jpg "Summary of Image 5"
[image24]: ./output_images/Summary_6.jpg "Summary of Image 6"
[image25]: ./output_images/Summary_7.jpg "Summary of Image 7"
[image26]: ./output_images/Summary_8.jpg "Summary of Image 8"


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

![alt text][image1]

## Distortion Correction
To apply the distortion to the raw images the distortion coefficients which are calculated in the camera calibration will be used. 
The image below shows three of the test images before and after the distortion.

![alt text][image2]

![alt text][image3]



## Persective Transform
A perspective transform maps the points (source points) in a given image to different, desired image points(destination points) with a new perspective. Here we use the perspective transform in a bird’s-eye view that let’s us view a lane from above. This will be useful for calculating the lane curvature later on.

The figures below show the process steps of the perspective transformation. The red marks are showing the source points in the left image and the destination points in the the right and already warped image. 

First 

|![alt text][image4]

Second

![alt text][image5]


## Color transforms and thresholding
Lane lines sometimes are better to detect when the image is converted to another color space. The most common color spaces are grayscale, RGB and HSV. The next figures will show one image with difficult conditions for lane line finding (shadows and diffrent pavement colors) converted to diffrent color spaces. To take out the lane line information the color spaces will be thresholded so that they in the end give out a binary image. The thresholds were choosen by trail and error.

![alt text][image6]

![alt text][image7]

![alt text][image8]

The images in the figure show that the S channel is doing a robust job of picking up the lines under very different color and contrast conditions, while the other selections look messy. 
HIER WEITER BESCHREIBEN
r auch ok --mit rein?
sonst mit h channel werden die schatteb gut ausgeglichen und als cimbi siehts gut aus

![alt text][image9] 

## Gradient thresholding
The canny edge detections used in the first project of lane line detections finds all possible edges in an image which is a lot of information. To reduce the amount of infomation and still keep the important data imformation, the soble operator of the canny edge detection will be thresholded. The x sobel is doing in most conditions the better job. The have just one threshold for the x and y gradient, the magnitude of the gradients can be used. The magnitude combines the x and y sobels as an absolut value of gradient. The figure below shows the thresholded x and y sobel operator and the magnitude of the gradient.

![alt text][image10]


In case of lane lines we are only interested in edges of a particular orientation. With calculating the direction of the gradient by taking the inverse tangent of the y gradient devided by the x gradient. To sort out the unimportant directions of the gradient the result will be thresholded. The next images shows the thresholded direction of the gradient.

![alt text][image11]

## Combination of Color and Gradient Thresholding
To find the best thresholding and combination trail and error was used. In the and the final binary is a combination out of:

HIER NOCHG EINTRAGEB
  *R
  * S
  * H
  * mag
  * dir

After all thresholding there is a final binary image which will be used to detect the lane lines in the furter steps.

![alt text][image12]

##  Detect Lane Pixels
After applying calibration, thresholding, and a perspective transform to a road image, you should have a binary image where the lane lines stand out clearly. However,Now after applying calibration, thresholding, and a perspective transform to a road image  we have a binary image where the lane lines stand out clearly.  However, I still need to decide explicitly which pixels are part of the lines and which belong to the left line and which belong to the right line.

I first take a histogram along all the columns in the lower half of the image (figure below) to detect the peaks as starting points of the two lane lines.

![alt text][image13]

With the starting points of the histogramm I implemented sliding windows and fit a polynomial to the lanes. This technique is done by searching for the nonzero pixels in every single sliding window area and center the window according to the found nonzero pixels. The image shows the result of the sliding winwow technique.

![alt text][image14]

Having the information out of the sliding window calculation the searching window technique can be implemented
Now I know where the lines are I can implement the searching window technique fo the next frame of video so I don't need to do a blind search again, but instead I am going just search in a margin around the previous line position. The result of the searching window technique is shown in the image below.

![alt text][image15]


## Curvature and Vehicle Position
Now I have a thresholded image, where I have estimated which pixels belong to the left and right lane lines, and I have fit a polynomial to those pixel positions. With this information and converting x and y from pixels space to meters, the radius of curvature can be calculated. 
For this calculation the assumption is made that the camera  is mounted at the center of the car, such that the lane center is the midpoint at the bottom of the image between the two lines are detected. The offset of the lane center from the center of the image (converted from pixels to meters) is the distance from the center of the lane.


## Warp back
The lane line fit with a polynomial now need to warp back onto the original image. To do that the polynomial line fit will be warped back with the perspective transform using the inverse matrix of the same given source and destination points. The image below shows the result.

![alt text][image16]


## Visual Output
The visual output will show the result of the lane line detection in form of an colored area. The values of curvature and vehicle position will be shown as well. An example of the output is shown in the image below.

![alt text][image17]
![alt text][image18]

## Summary

![alt text][image19]
![alt text][image20]
![alt text][image21]
![alt text][image22]
![alt text][image23]
![alt text][image24]
![alt text][image25]
![alt text][image26]


## Discussion
 
 * Filtering not optimal
 * warped before color thresholding made it much better
 
Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?

Discussion includes some consideration of problems/issues faced, what could be improved about their algorithm/pipeline, and what hypothetical cases would cause their pipeline to fail.
 
