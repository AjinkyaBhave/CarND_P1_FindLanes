# **Finding Lane Lines on the Road** 

## Ajinkya Bhave

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps: 
1. Convert image to grayscale
2. Apply Gaussian blur to remove noise
3. Apply Canny edge detection to obtain all edges
4  Apply crop mask to focus only on lower trapezoidal portion of image
5. Apply Hough transform to detect all line segments from edges in cropped region
5. Filter left and right line segments that belong to lane lines on the basis of slope statistics
6. Fit a line to the filtered line segments using np.polyfit
7. Draw left and right lane lines by extrapolating the beginning and end points using the calculated line coefficients

In order to draw a single line on the left and right lanes, I modified the draw_lines function as follows:
1. Calculated the slope for each detected line segment from Hough transform
2. Separated into left and right line slopes on basis of sign 
3. Filtered outlier line segments on basis of range of slope values
4. Stored valid left and right line segments and obtained mean and standard deviation of slopes
5. Filtered line segments based on 1 std. deviation from mean
6. Fit a line to the final set of left and right line segments using np.polyfit
7. Drew a line for left lane assuming starting at left bottom of image and going to the left top of trapezoidal mask region
8. Drew a line for right lane assuming starting at right bottom of image and going to the right top of trapezoidal mask region
9. Saved valid left and right line segment coordinates for possible use in next frame. This is useful in case the algorithm does not find    valid segments in next image due to noise/lighting/curvature.


### 2. Identify potential shortcomings with your current pipeline


Currently, the pipeline works well on straight lines. However, it can struggle with sharp curves and bad lighting. So, it's not very robust and I would not put it in my car as is. 
There are multiple parameters and thresholds to tune manually (Canny, Hough, slope outliers, mask region). This makes the approach sensitive to the test images and videos we have. I am not convinced about its generalisation to other test videos or roads.
It is not real-time, or at least, it will struggle on an embedded processor. I know the object of this project is to understand the fundamental algorithms and trade-offs, not production code, but this is a potential issue I would foresee if I had to adapt it to practical implementations. I have seen other course members using clustering, more data-intensive curve fitting, and convoluted logic to get the algorithm more robust. I have purposely stayed away from all these methods because I wanted to find something that can potentially run in real-time.


### 3. Suggest possible improvements to your pipeline

The current approach lacks a good lane and camera model. So the first improvement would be to apply some polynomial assumption to the lane markings, possible a clothoid, to be able to detect line segments more robustly. 
Tracking each line over multiple frames using a Kalman filter or similar approach would also increase robustness, especially when adverse conditions make the algorithm miss or lose lanes intermittently. I foresee this being very useful in rain, snow, sensor malfunction.
Changing from RGB to HSV space would make it more insensitive to lighting changes. I did not have time to explore this during P1 but will try to do it later or during P4.
