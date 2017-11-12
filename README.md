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

[image1]: ./output_images/Undistorted.jpg "Undistorted"
[image2]: ./output_images/Undistorted_and_Warped_Image.jpg "Road Transformed"
[image3]: ./output_images/Threshold.jpg "Binary Example"
[image4]: ./output_images/ProcessedImage.jpg "Output"
[video1]: ./project_video.mp4 "Project Video"
[video2]: ./challenge_video_output.mp4 "Challenge Video"
[video3]: ./harder_challenge_video_output.mp4 "Harder Challenge Video"

### Camera Calibration

The code for this step is contained in the first code cell of the IPython notebook located in "Advanced Lane Lines.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `object_points` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `image_points` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `object_points` and `image_points` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Distortion correction

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Thresholds

I used a combination of color thresholds in 2 color spaces, hls and lab. Here is an example of an image where thie se thresholds have been performed:

![alt text][image3]

#### 3. Trasforming Images

The code for my perspective transform includes a function called `warp()`, which appears in the 4th code cell of the IPython notebook. The `warp()` function takes as inputs an image (`image`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32([(575,464),
                  (707,464), 
                  (258,682), 
                  (1049,682)])
dst = np.float32([(450,0),
                  (830,0),
                  (450,720),
                  (830,720)])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 575, 464      | 450, 0        | 
| 707, 464      | 830, 0        |
| 258, 682      | 450, 720      |
| 1049,682      | 830, 720      |

I use 2 funcitons to fit 2nd degree polynomials - sliding_window_polyfit and polyfit_using_prev_fit. 
First is for first time deteciton and the second is building on already found fits.
These funciton can be found in code cells 24 and 25.

The radius and the distance to the center is computed in function calc_curv_rad_and_center_dist in code cell 26.
All other drawing is done in draw_line and draw_data in cell 27.
All of this is bundled together in function process_image in cell 28.
This function is also used for processing the video output.
Example for the whole process image pipeline can be seen here:

![alt text][image4]

---

### Pipeline (video)

#### 1. Link to the final project outout video
Here's a link to the ![Project Video][video1]

#### 2. Link to the final challenge outout video
Here's a link to the ![Challenge Video][video2]

#### 3. Link to the final harder challenge outout video
Here's a link to the ![Harder Challenge Video][video3]

---

## Discussion

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

First I tried various threshold methods to select for the lanes.
The ones selected in the project worked the best for this project.
But as seen in the harder challenge video, they do have shortcomings.
These threshold proably need to be explored further.

Also the source and destination points used for warping the images need to be dynamic and not hard coded.
