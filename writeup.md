## Advanced Lane Finding Project Writeup

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

[image1]: ./model_images/output_5_0.png "Undistorted"
[image2]: ./model_images/output_6_0.png "Road Transformed"
[image3_1]: ./model_images/output_9_0.png "Binary Example"
[image3_2]: ./model_images/output_10_0.png "Binary Example"
[image3_3]: ./model_images/output_12_0.png "Binary Example"
[image3_4]: ./model_images/output_14_0.png "Binary Example"
[image3_5]: ./model_images/output_15_0.png "Binary Example"
[image3_6]: ./model_images/output_18_0.png "Binary Example"
[image3_7]: ./model_images/output_19_0.png "Binary Example"
[image4]: ./model_images/output_24_0.png "Warp Example"
[image5]: ./model_images/output_30_1.png "Fit Visual"
[image6]: ./model_images/output_36_0.png "Output"
[video1]: ./advanced_lane_line.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration


The code for this step is contained in the first three code cell of the IPython notebook located in "./model.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps below).  Here's an example of my output for this step.

![alt text][image3_1]
![alt text][image3_2]
![alt text][image3_3]
![alt text][image3_4]
![alt text][image3_5]
![alt text][image3_6]
![alt text][image3_7]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warped()`, which appears in the 17th code cell of the IPython notebook (./model.ipynb).  The `warped()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
    src = np.float32([[688,448],
                     [1112,716],
                     [212,716],
                     [593,448]])
    dst = np.float32([[990,0],
                     [990,720],
                     [320,720],
                     [320,0]])
```

Warped image example below.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial in the 21st code cell of the model.ipynb :

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the 25th code cell of the model.ipynb

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in in the 28th code cell of the model.ipynb.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

When road has more light my model become weaker due to hls/gradient implementation. I can change this depend on general color saturation for improve my model. 
