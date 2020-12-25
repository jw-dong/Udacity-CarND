## **Advanced Lane Finding Project**

In this project, a pipeline was programmed to identify the lane lines in the given images and video. The goals / steps of this project are the following:

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
[image3]: ./examples/binary_combo_example.jpg "Combined Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Color Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video_solution.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Pipeline (image)

### Step 1: Compute the camera calibration using chessboard images

The code for this step is contained in the "Step 1: Compute the camera calibration using chessboard images" code cell of the IPython notebook located in "./Advanced Lane Finding Project.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Step 2: Apply a distortion correction to the raw images

The code for this step is contained in the "Step 2: Apply a distortion correction to the raw images" code cell of the IPython notebook located in "./Advanced Lane Finding Project.ipynb".

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

### Step 3: Use color transforms, gradients, etc., to create a thresholded binary image

The code for this step is contained in the "Step 3: Use color transforms, gradients, etc., to create a thresholded binary image" code cell of the IPython notebook located in "./Advanced Lane Finding Project.ipynb".

In this step, the following funtions are defined to calculate several gradient measurements (x, y, magnitude, direction and color).

- (Line #02-18) Calculate directional gradient: `abs_sobel_thresh()`. 
- (Line #21-36) Calculate gradient magnitude: `mag_thres()`. 
- (Line #39-55) Calculate gradient direction: `dir_thresh()`.
- (Line #58-69) Calculate color threshold: `col_thresh()`.

Here's an example of my output for this step.

![alt text][image3]

### Step 4: Apply a perspective transform to rectify binary image ("birds-eye view")

The code for my perspective transform includes a function called `warp()`, which appears in lines 2 through 28 in the "Step 4: Apply a perspective transform to rectify binary image ("birds-eye view")" code cell of the IPython notebook located in "./Advanced Lane Finding Project.ipynb".

The `warp()` function takes as inputs an image (`img`), as well as source (`src_coordinates`) and destination (`dst_coordinates`) points. I chose the hardcode the source and destination points in the following manner:

```python
    src_coordinates = np.float32(
            [[280,  700],  # Bottom left
             [595,  460],  # Top left
             [725,  460],  # Top right
             [1125, 700]]) # Bottom right

    dst_coordinates = np.float32(
            [[250,  720],  # Bottom left
             [250,    0],  # Top left
             [1065,   0],  # Top right
             [1065, 720]]) # Bottom right 
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 280, 700      | 250, 0        | 
| 595, 460      | 250, 720      |
| 1125, 460     | 1065, 0       |
| 725, 700      | 1065, 720     |

I verified that my perspective transform was working as expected by drawing the `src_coordinates` and `dst_coordinates` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

### Step 5: Detect lane pixels and fit to find the lane boundary

The code for this step is contained in the "Step 5: Detect lane pixels and fit to find the lane boundary" code cell of the IPython notebook located in "./Advanced Lane Finding Project.ipynb".

Here, I first calculated the histogram over the combined warp image from Step 4. I, then, applied the polynomial fitting and sliding windows to find the lane lines using the results from histogram function as the starting points. 

The fitted lane lines with a 2nd order polynomial looks like this:

![alt text][image5]

### Step 6: Determine the curvature of the lane and vehicle position with respect to center

The code for this step is contained in the "Step 6: Determine the curvature of the lane and vehicle position with respect to center" code cell of the IPython notebook located in "./Advanced Lane Finding Project.ipynb".

The curvatures of the lane lines are calculated using the function `measure_curvature_pixels()`. 

### Step 7: Warp the detected lane boundaries back onto the original image

The code for this step is contained in the "Step 7: Warp the detected lane boundaries back onto the original image" code cell of the IPython notebook located in "./Advanced Lane Finding Project.ipynb".

The lane lines and their boundaries are identified and overlaid back to the original image. 

### Step 8: Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position

The code for this step is contained in the "Step 8: Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position" code cell of the IPython notebook located in "./Advanced Lane Finding Project.ipynb".

The lane lines and their boundaries are identified and overlaid back to the original image. The offset of the vehicle's position to the center of the image is calculated. Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

The code for this step is contained in the "Step 9: Run pipeline in a video" code cell of the IPython notebook located in "./Advanced Lane Finding Project.ipynb".

All the funtions from the image pipeline have been wrapped in a Class `ProcessImage`. Each frame of the video will be processed to identify the lane lines. 

Here's a [link to my video result](./project_video_solution.mp4)

---

### Discussion

### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Overall, the pipeline works well. However, at certain curvature of the lanes, the pipeline identified the wrong curvature and generate misleading polynominal fitting of the lanes. Increase the margin value in the lane detection function may resolve this issue. 