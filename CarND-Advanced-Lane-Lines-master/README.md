
---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

The goal of this project is to develop a pipeline to process a video stream from a forward-facing camera mounted on the front of a car, and output an annotated video.

These are the following steps:

###Step 1: Apply distortion correction using a calculated camera calibration matrix and distortion coefficients.
###Step 2: Apply a perspective transformation to warp the image to a birds eye view perspective of the lane lines.
###Step 3: Apply color thresholds to create a binary image which isolates the pixels representing lane lines.
###Step 4: Identify the lane line pixels and fit polynomials to the lane boundaries.
###Step 5: Determine curvature of the lane and vehicle position with respect to center.
###Step 6: Warp the detected lane boundaries back onto the original image.
###Step 7: Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

###Step 1:
Here, I utilized the functions findChessboardCorners() and drawChessboardCorners() to find the locations of corners in the given set of pictures of a chessboard taken from many angles. Next,  the chessboard corners were used by the function calibrateCamera to get the camera calibration matrix and distortion coefficients. Then, the camera calibration matrix and distortion coefficients were used with the function undistort to remove distortion from highway images.

![alt text][image1]

###Step 2:
The goal of this step is to transform the undistorted images to a "birds eye view" of the road which focuses only on the lane lines and displays them in such a way that they appear to be  parallel to eachother. To achieve the perspective transformation I used the functions getPerspectiveTransform and warpPerspective which take a matrix of four source points on the undistorted image and remaps them to four destination points on the warped image.


![alt text][image2]

###Step 3:
In this step I attempted to convert the warped image to different color spaces and create binary thresholded images which highlight only the lane lines and ignore everything else. 
I created a combined binary threshold to create one combination thresholded image which does a great job of highlighting almost all of the white and yellow lane lines in an image (discussed in the udacity online videos).
![alt text][image3]


###Step 4,5,&6:

At this point I was able to use the combined binary image to isolate only the pixels belonging to lane lines. The next step was to fit a polynomial to each lane line. After fitting the polynomials I was able to calculate the position of the vehicle with respect to center with the following calculations:

Calculated the average of the x intercepts from each of the two polynomials position = (rightx_int+leftx_int)/2

Calculated the distance from center by taking the absolute value of the vehicle position minus the halfway point along the horizontal axis distance_from_center = abs(image_width/2 - position)

If the horizontal position of the car was greater than (width of image/2) than the car was considered to be left of center, otherwise right of center. Finally, the distance from center was converted from pixels to meters by multiplying the number of pixels by 3.7/700. Next I  calculated the radius of curvature for each lane line in meters. The final radius of curvature was taken by average the left and right curve radiuses.

Source: http://www.intmath.com/applications-differentiation/8-radius-curvature.php

![alt text][image4]
![alt text][image5]
![alt text][image6]

---
###Step 7
The final step in processing the images was to plot the polynomials on to the warped image, fill the space between the polynomials to highlight the lane that the car is in, use another perspective trasformation to unwarp the image from birds eye back to its original perspective, and print the distance from center and radius of curvature on to the final annotated image.

###Pipeline (video)
I established a pipeline to process the images, the last phase was to have the pipeline to process videos frame-by-frame, to simulate what it would be like to process an image stream in real time on an actual vehicle. The link below is the result of my pipeline running on the test video provided for the project.
Here's a [link to my video result](./project_video.mp4)

---




