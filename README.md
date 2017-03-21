# **Finding Lane Lines on the Road** 

## Writeup - Ron Danon

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:  
* Make a pipeline that finds lane lines on the road  
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/colorThreshold_solidWhiteCurve.jpg
[image2]: ./test_images_output/gray_solidWhiteCurve.jpg
[image3]: ./test_images_output/blur_solidWhiteCurve.jpg
[image4]: ./test_images_output/edges_solidWhiteCurve.jpg
[image5]: ./test_images_output/maskedEdges_solidWhiteCurve.jpg
[image6]: ./test_images_output/lines_solidWhiteCurve.jpg
[image7]: ./test_images_output/final_solidWhiteCurve.jpg

---

### Reflection

### 1. Summary of my pipeline.

My pipeline is consisted of 7 image processing steps.  
The methods which were applied to each image in chronological order are:  
	a. Colors threshold - ignoring some pixels based on their RGB values.  
	b. Convert colors threshold image to gray scale.  
	c. Applying gaussian filter on gray scale image.  
	d. Using Canny method to find image edges on the filtered image.
	e. Masking the edges image to the interest area.    
	f. Using Hough transform method on the masked edges image to find straight lines.  
	g. Converting the lines to a two single lines on the left and right lanes (applied with the finalLines function - I didn't modify the draw_lines() function).  

The parameters of each method were chosen at first with the help of slider widgets in order to save time. The parameters were chosen with the combination of logic and trial and error. Then the parameters were fine-tuned in order to yeild better results for the videos.  

(Note: The mask parameters were tuned first, but the mask was applied only after the Canny method).  

Most of the image processing methods parameters are based on the image shape so they would fit any image resolution.

#### Explanation of the lines convertion
The slope of each Hough line was calculated. Assuming the two lane lines will always have one positive slope line and one negetive slope line and that they will always be in a simillar posture,  only the lines with a slope between -0.5 and -2 or 0.5 and 2 were taken into acount. Then the mean slope (for each line - positive and negative) was calculated.  
The yMax of the two lines was set to be at the bottom of the image. The yMin of the two lines was set to be the minimum of all of the y values of the Hough lines that were found.  
All we miss now is x0 and x1 of each lane line.  
For the positive slope line, an array of possible x1 values was calculated by using the array of x0 values of the positive slope Hough lines, then the x1 was taken as the mean of that array. Lastly, x0 was calculated using all the other data (slope, yMin = y0, yMax = y1, and x1).  
Simillarly, the negetive slope line was found, but first x0 was found and then x1.

Here are images showing the procedure of the pipeline:  
a.![alt text][image1]  
b.![alt text][image2]  
c.![alt text][image3]  
d.![alt text][image4]  
e.![alt text][image5]  
f.![alt text][image6]  
g.![alt text][image7]  


### 2. Identify potential shortcomings with your current pipeline

Some shortcoming I though of are:  

a. Some very different lane color should cause problems (for example red).  
b. If the car is making a turn, or for some reason is not aligned with the lanes, the slope of the line will probably go out of y difined range of slopes.  
c. A strong curve in the road can cause problems with the straight lines detection (for the specific parameters i chose).  
d. Any dirt on the lines (like snow, or sand) will prevent line detection.  
e. Near by cars in the front of the camera, or cars that change lanes can prevent line detection.  


### 3. Suggest possible improvements to your pipeline

Some improvements I though of are: 

a. Averaging the current found line with the previous line (kind of filtering on the calculated lane line).  
b. In cases were no lane line is found, use the last found lane line.  
c. Using more conservative parameters (i.e. less Hough lines would be found). If no lane lines found, then use less conservative paramaeters... continue until the lane lines are found or until reached the finale parameters set.  
d. Adjusting the parameters on the fly based on the found lane lines. For example, nurrowing the slope range, addjusting the colors thresholds, changing he mask area, etc.  
