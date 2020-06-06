# Advance Lane Detection Project 

## Goals and Objectives

------

The steps of this project are the following:

- Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
- Apply a distortion correction to raw images.
- Use color transforms, gradients, etc., to create a thresholded binary image.
- Apply a perspective transform to rectify binary image ("birds-eye view").
- Detect lane pixels and fit to find the lane boundary.
- Determine the curvature of the lane and vehicle position with respect to center.
- Warp the detected lane boundaries back onto the original image.
- Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

### Camera calibration

I started by Preparing Objpoints and ImagePoints. The following steps were followed-:

1. Grayscale the image

2. Find Chessboard Corners. It returns two values ret,corners. ret stores whether the corners were returned or not

3. If the corners were found, append corners to image points.

4. I have also drawn the chessboard corners to visualize the corners

5. With this step  to get image points and object points which will be required to calculate the camera calibration and distortion coefficients.

6. run calibrateCamera function and returns a bunch of parameters and obtain the camera matrix (mtx) and distortion coefficient (dist).

7. use camera matrix and distortion coefficient to undistort  image.

![](https://raw.githubusercontent.com/aaron7yi/CarND-Advanced-Lane-Lines/master/Example_of_undistorted_calibration_image.png)
## Perspective Transform

In this step, I first defined a source points and destination points, the next step is to calculate a Matrix with the source and destination points. The destination points were selected appropriately so as to see a good bird's eye perspective.  Then use CV2 warpPerspective function to get the final warped image.



![](https://raw.githubusercontent.com/aaron7yi/CarND-Advanced-Lane-Lines/master/Example_of_birds_eye_view.png)




## BinaryPipeline
I started by Preparing BinaryPipeline. The following steps were followed-:
1. copy raw image

2. apply undistorion to the image

3. apply perspective transform to the image

4. extraction R & S from different color space

5. apply threshold to obtains binary color image

6. run sobel algorithm and obtain sobel image

7. combined three binary images together

   ![](https://raw.githubusercontent.com/aaron7yi/CarND-Advanced-Lane-Lines/master/Example_binary_images.png)

## Histogram Algorithm
1. sum up the array values  of the bottom half image in vertical (y) direction

2. find peaks in this statistic histogram diagram

   ![](https://raw.githubusercontent.com/aaron7yi/CarND-Advanced-Lane-Lines/master/histogram_diagram.png)

## Sliding Windows Algorithm

The sliding window is applied in following steps:

1. The left and right base points are calculated from the histogram

2. calculate the position of all non zero x and non zero y pixels

3. iterating over the windows where we start from points calculate 

4. identify the non zero pixels in the window we just defined

5. collect all the indices in the list and decide the center of next window using these points

6. separate the points to left and right positions

7. fit a second degree polynomial using np.polyfit

8. virtualizing lanes, windows and points

   <img src="https://raw.githubusercontent.com/aaron7yi/CarND-Advanced-Lane-Lines/master/Example_of_virtualizing_sliding_windows.png" style="zoom: 33%;" />

## Margin Searching Algorithm

1. use previous polynomial lines to generate margin

2. searching points in the margin area

3. fits these points in a new polynomial line

4. virtualizing results

   <img src="https://raw.githubusercontent.com/aaron7yi/CarND-Advanced-Lane-Lines/master/Example_of_virtualizing_margin_searching.png" style="zoom:33%;" />
## Drawing lanes Algorithm
this algorithm first drawing line and driving zone on the warped image, then execute the inverse prospective transform to convert "eye bird view" to original camera view.

## Measuring Curvature Algorithm
1. convert from pixels to real-world meter measurements

2. use the formula below to calculate the radius of curvature 

   <img src="https://raw.githubusercontent.com/aaron7yi/CarND-Advanced-Lane-Lines/master/curvature_measure_equation.png" style="zoom: 67%;" />

3. assume camera mounted in the middle of the vehicle, then use two bottom points to 	calculate the distance of the middle line and real-time car position


## Image Pipeline
I started by Preparing FinalPipeline. The following steps were followed-:
1. convert BGR to RGB

2. apply undistorion to the image

3. apply binaryPipeline to the image

4. run sliding windows algorithm

5. run drawing lanes algorithm

6. calculate the radius of curvature

7. put text on the image

8. scale real time binary image and add it on the image 

9. convert BGR to RGB

   <img src="https://raw.githubusercontent.com/aaron7yi/CarND-Advanced-Lane-Lines/master/Example_of_final_proccessed_image.png" style="zoom: 67%;" />

## Video Pipeline 

  The pipeline used for the video was the same one described above. 

## Discussion
The code does not perform quite well on when the shadow appear near the driving lance. Further parameters tuning and  image filtering methods are suggested to be utilized in order to process challenge videos.  The project also made me realize the significance of Machine Learning and Deep learning based method. 