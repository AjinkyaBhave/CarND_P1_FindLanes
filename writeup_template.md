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

My pipeline consisted of 5 steps. 
1. Convert image to grayscale
2. Apply Gaussian blur to remove noise
3. Apply Canny edge detection to obtain all edges
4  Apply crop mask to focus only on lower trapezoidal portion of image
5. Apply Hough transform to detect all line segments from edges in cropped region
5. Filter left and right line segments that belong to lane lines on the basis of slope statistics
6. Fit a line to the filtered line segments using np.polyfit()
7. Draw left and right lane lines by extrapolating the beginning and end points using the calculated line coefficients

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by
1. Calculated the slope for each detected line segment from Hough transform
2. Separated into left and right line slopes on basis of sign 
3. Filtered outlier line segments on basis of range of slope values
4. Stored valid left and right line segments and obtained mean and standard deviation of slopes
5. Filtered line segments based on 1 std. deviation from mean
6. Fit a line to the final set of left and right line segments using np.polyfit()
7. Drew a line for left lane assuming starting at bottom of image and going to the end of trapezoidal mask region
8. Drew a line for right lane assuming starting at bottom of image and going to the end of trapezoidal mask region
9. Saved valid left and right line segment coordinates for possible use in next frame. This is useful in case the algorithm does not find    valid segments in next image due to noise/lighting/curvature.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
