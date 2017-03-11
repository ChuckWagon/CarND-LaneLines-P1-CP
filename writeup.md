# **Finding Lane Lines on the Road** 

## Written Reflection

**Udacity Self-Driving Car Nano Degree - Term 1 - Project 1**

[//]: # (Image References)

[image1]: /examples/solidYellowCurve-bf.jpg "Original Image"
[image2]: /examples/solidYellowCurve-mid0.jpg "Gray Scale"
[image3]: /examples/solidYellowCurve-mid1.jpg "Gaussian Blur"
[image4]: /examples/solidYellowCurve-mid2.jpg "Canny"
[image5]: /examples/solidYellowCurve-mid3.jpg "Region Section"
[image6]: /examples/solidYellowCurve-mid4.jpg "Region Output"
[image7]: /examples/solidYellowCurve-mid7.jpg "Hough transform"
[image8]: /examples/solidYellowCurve-mid8.jpg "Hough transform overlay"
[image9]: /examples/solidYellowCurve-mid9.jpg "Extrapolate Lines"
[image10]: /examples/solidYellowCurve-mid10.jpg "Final Image"


### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.


My pipeline consisted of the following seven (7) steps:


1. The image is converted to grayscale in order to decrease the complexity of the image from three (3) channels (red, green and blue) ranging from 0 to 255 per pixel to one (1) channel (grayscale) ranging from 0 to 255 per pixel. 

   Original Image:

   ![alt text][image1]

   Grayscale Image:

   ![alt text][image2]

2. A Gaussian blur is performed on the image to "smooth" it out and reduce tiny imperfections in the image.

   Gaussian Blur:
   
   ![alt text][image3]

3. The smoothed image is put through Canny Edge Detection, which uses clever algorithms to find and highlight all of the points along edges in the image and blackout everything else. 

   Canny Edge Detection:

   ![alt text][image4]

4. A region of interest function is run on the image, which keeps everything inside a pre-defined polygon on the image and converts everything outside of the region to black.

   Region of Interest:

   ![alt text][image5]   

   Region of Interest Function applied to Canny image.

   ![alt text][image6]

5. A Hough transform is run on the image, which identifies straight lines based on criteria laid out in the functions parameters.   

   Hough Transform:

   ![alt text][image7]   

   Overlaid on Original Image:

   ![alt text][image8]

6. The slope and xy coordinates of the various lines returned from the Hough transform are averaged together, applying greater weight to the longer length lines. The left lane line and right lane line are differentiated by separating the positive sloped lines (right line) from the negative sloped lines (left line). The extrapolated data is used to draw red lines from the base of the image to the horizon (y-coordinate: 350).   

   Extrapolated Lines:

   ![alt text][image9]
  
7. The red lane lines are overlaid onto the original image.  

   Extrapolated Lines Overlaid on Original Image:

   ![alt text][image10]


### 2. Identify potential shortcomings with your current pipeline

1. Curves in the road do not work well for two reasons:
  * In the current pipeline all positive sloped lines are attributed to the right lane line and all negative sloped lines are attributed to the left lane line. A dramatic curve to the right could easily push both lane lines to have a negative slope or vice versa.   
  * The current pipeline calculates a straight line using y = mx + b. This is inaccurate when the lane lines are curved. 
2. Multiple lane lines caused by lane markings (such as bike lanes), as well as road imperfections (such as vertical lines created from overlaid asphalt or spaces between concrete slabs), would create inaccurate readings in the current pipeline.
3. A single lane that splits into two (2) or more lanes, typically due to left or right turn lanes, would create issues if the current pipeline were being used to steer a vehicle.    
 
### 3. Suggest possible improvements to your pipeline

1. The issues raised above, in #1 (part a) and #2, could be avoided if the detection system optimized for lane lines that were a fixed width apart. 
2. Issue #1 (part b) could be improved using the quadratic formula (y = ax^2 + bx + c) to detect and draw curved lines. 
3. Issue #3 would likely need to utilize additional inputs (such as road signs and GPS instructions) but could be helped by the system being triggered to look ahead and expect a lane to split into multiple lanes when the lane lines immediately in front of the car begin to diverge in different directions.  
