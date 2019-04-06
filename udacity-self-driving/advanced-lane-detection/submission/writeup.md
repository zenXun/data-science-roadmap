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


## Pipeline (single frame)

### Camera Calibration

The code for this step is contained in cells 1 through 3 in IPython notebook located in "./submission/P2.ipynb". 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

[image1]: ../test_images/test1.jpg "Original Image"
[image2]: undistorted_test1.png "Undistorted Image"

### Color Transformation

The code for this step is contained in cell 4. 

I start by converting the image to HLS color space. Then I choose to use S channel of HLS color space and Sobel X as the two variables to perform the thresholding. The thresholds I choose for these two variables are (170, 255) and (20, 100) respectively. 

After thresholding gradient X and color channel, I obtained this result:

[image3]: undistorted_test1.png "Original Image after Undistortion"
[image4]: binary_img.png "Binary Image"

### Perspective Transformation

The code for this step is contained in cell 5.

I start by defining the interested area which is a ladder-shaped area surround the lane. Then I get Matrix M and Minv using the `cv2.getPerspectiveTransform()` function. I applied M to `cv2.warpPerspective()` to get warped perspective and obtained the result:

[image5]: original_img_with_area.png "Undistorted Image with Interested Area"
[image6]: warped_img_with_area.png "Undistorted Image with Interested Area (Bird-eye view)"

### Lane Boundary Detection

The code for this step is contained in cells 6 through 9. Here I leverage 3 ways of sliding windows to draw the lane boundary. I use sliding window I in the pipeline. 

Sliding window I can be used when there's no fitted line data in the interation processing the previous frame. It starts from the basis of two peaks in the histogram and moves gradually upward containing the majority of points in the center of the window. 

The result is:

[image7]: sliding_window_I.png "Lane Boundary Detected using Slide Window I"

### Curvature Computation and Vehicle Relative Position

The code for this step is contained in cell 10. 

I compute curvature using the formula in https://www.intmath.com/applications-differentiation/8-radius-curvature.php. In order to get the distance in the real world, I multiple the ratio of 30/720 (meters per pixel in y dimension) for y values and 3.7/700 (meters per pixel in x dimension) for x values respectively. 

I compute the vehicle position by calculating the difference between the center of two lane boundary and the center of the frame. 

### Lane Boundary Plot

The code for this step is contained in cells 11 through 12. 

I start with fill the lane area with green color using `cv2.fillPoly()` function. Then I use the Minv got from the perspective transformation phase to revert the warped img back to original perspective. Finally, I use `cv2.addWeighted()` function to overlay the lane boundary image onto the original road image. The result is:

[image8]: lane_boundary.png "Lane Boundary on Original Road Image"
[image9]: lane_boundary_with_info.png "Lane Boundary with Information"


## Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

[video1]: ./video_output.mp4 "video_output"
[video2]: ./challenge_video_output.mp4 "challenge_video_output" 
[video3]: ./harder_challenge_video_output.mp4 "harder_challenge_video_output"
---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

When I was testing my pipeline on hard_challenge_video, I found the lane detection performed poorly. From my understanding, the reason is because the complex environment including shadows on the roads and constant turning of the road etc. These factors challenge the robustness of edge detection significantly. Hence, if I could explore this topic further, I will try to make a more adaptive edge detection in terms of dynamic parameters like thresholds. What's more, I would also want to apply deep learning to tackle this task. 