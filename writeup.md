# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)


[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./writeup_images/gsi.png "Grayscale"
[image3]: ./writeup_images/ced.png "Canny Edge Detection"
[image4]: ./writeup_images/roi.png "ROI"
[image5]: ./writeup_images/htd.png "Hough Transform (Detected)"
[image6]: ./writeup_images/fid.png "Final Image (Detected)"
[image7]: ./writeup_images/hte.png "Hough Transform (Extrapolated)"
[image8]: ./writeup_images/fie.png "Final Image (Extrapolated)"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following 5 steps: 
1. First, I converted the images to grayscale.

![alt text][image2]

2. Then I applied a gaussian blur, followed by canny edge detection, resulting in the following image.

![alt text][image3]

3. A simple mask was applied next to the image.

![alt text][image4]

4. Then I applied a hough transform  to convert the edges into the lines.

![alt text][image5]

Overlaid on the original image looks like this:

![alt text][image6]

5. In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first creating arrays to represent the left lane marking and the right lane marking.
I then sorted each line generated from the hough transform based on positive or negative slope and appended them to the left lane and right lane respectively.
Then I used a simple linear fit function (numpy polyfit) to each of these arrays to find the best fit line.  With the resulting slope and y-intercept values, I could determine each lane marker in x- and y-coordinates

Those lines are shown below.

![alt text][image7]

Overlaid on the original image looks like this:

![alt text][image8]

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the lane markings are not linear.  The simple line fit that I used cannot handle any lines with curvature. 

Another shortcoming could be the masked region on the image.  If the road topology changes or vehicles starts a turn, the mask will not adapt.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to modify the fitting algorithm to handle curvature.  Maybe it could be fitted with splines

Another potential improvement could be to adapt the mask based on the image.  It might be possible to use other lanes or edge geometry (curb/guard rail) to predict where a more appropriate mask could be used.
