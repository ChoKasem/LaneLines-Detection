# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road images
* Apply the pipeline to video and track the lane lines


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/solidWhiteCurve.jpg "Image1"

---

### Reflection

### 1. Pipeline Description

My pipeline consisted of 5 steps.

1. Convert to Grayscale

   Converting to grayscale is done using cv2.cvtColor(img, cv2.COLOR_RGB2GRAY) function. By converting to grayscale, it facilitate image processing process.
   
   ![alt text][image1]

2. Apply Canny Edges

    Identifying the edges is done using cv2.Canny(img, low_threshold, high_threshold) function to find all the edges in the images. The edges is the area where the gradient is significant.

3. Add Gaussian Blur

    Adding Gaussian Blur using cv2.GaussianBlur(img, (kernel_size, kernel_size), 0) remove all the small noise and error that could occur when trying to identify the line.

4. Draw Hough Line in the Region of Interest

    For this part of the pipeline, there are two helper functions involve. The first one is for identifying the line in Hough Space by using cv2.HoughLinesP(img, rho, theta, threshold, np.array([]), minLineLength=min_line_len, maxLineGap=max_line_gap) function. The output of the function give the list of two points which form the lines. The parameter of the hough line are number of threashold which tell minimum voting needed, the minimum line length, and hte max line gap. These parameters are tune to best identify the line.

    The second helper function is for drawing line. This is done originally with cv2.line(img, (x1, y1), (x2, y2), color, thickness). Later on, to take into account possiblity of very curve road by implementing polynomial regression. First, I group the point in the left and right lane by using the slope of the lines. After that, I use numpy polyfit and poly1d function to find the polynomial regression function. Then I can find the points using that function, and draw the line connecting each points with cv2.polylines. After experimenting, I decided to use polynomial of degree 1 (which is linear regression) because it is the most robust and provide most accurate result.

5. Combine the Line Image with Original Image

   Lastly, using the cv2.addWeighted(initial_img, α, img, β, γ), I combine the two image.

The resulting images are shown below:

![Image after Pipeline][image2]


### 2. Potential Shortcomings


One potential shortcoming would be what would happen when the line have high curvature. Currently my code is using linear regression. If the curve of the road is too high, linear result would be inaccurate.

Another shortcoming could be when the marking on the road spread out too far or it disaapear when marking fade too much. This would make the program to lose track of the road.


### 3. Possible Improvements

A possible improvement would be to incorporate the previous result. Therefore, whenever the car is in the region where the marking fade at certain region, it would use the previous data to predict where the lane is. This would require some probabilistic robotics technique.

Another potential improvement could be to tune the polynomial function to find the one that best fit the data when the curvature is really high.
