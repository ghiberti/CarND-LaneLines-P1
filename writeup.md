# **Finding Lane Lines on the Road** 

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

First step in my pipeline is to get a grayscale copy of the original image. Then the image is put through Gaussian blur with kernel size 3 before it is sent to Canny Edge detection. I used 100 and 200 respectively for lower and higher tresholds for Canny function.

![Canny Output](https://github.com/ghiberti/CarND-LaneLines-P1/blob/master/examples/CannyOut.png)

Next a region of interest is extracted from the result of Canny. For the project I used hard coded coordinates for polygon vertices and applied these as a mask using the 'region of interest' helper function.

![Region of Interest](https://github.com/ghiberti/CarND-LaneLines-P1/blob/master/examples/ROI.png)

Finally, the region of interest is put through HoughLinesP function to extract lane lines. I achieved the best result by setting 'theta' to 60 degrees and both 'min_line_length' and 'max_line_gap' to zero. This resulted in more granular lines as almost all them appeared to be a pixel wide.

![Result](https://github.com/ghiberti/CarND-LaneLines-P1/blob/master/examples/result.png)

In order to mark the full extent of the lane on the road, I figured I would need the X coordinates at extreme ends since Y dimension would be constraint by ROI. Thus extended the draw function to separate the lines returned by Hough Transform as left or right depending on the X coordinates. Then I calculated the min and max values for X coordinates on each side and slopes and intercepts of the lines that must connect them. Finally plotted this line on the image using the Y coordinates of the ROI.

### 2. Identify potential shortcomings with your current pipeline

White stains on the road does throw of the extreme coordinate parameterazition as it shifts the min or max extreme coordinates depending on the side. (eg. 14 secs into solidYellowLeft.mp4)

Steep curves confuse the right and left sorting of the lanes due to ROI being derived from fixed points on the image plane.

Parallel markings by the side of the road also fools the algorithm into thinking that they are lane lines.


### 3. Suggest possible improvements to your pipeline

Adapting the region of interest to the curvature of the road.

I feel that I have overfitted the HoughTransformation to the problem at hand. My algorithm's failure to generalize to optional challange video is a proof for this. Therefore a Hough Transform with better tuned parameters could improve the algorithm to handle more variety of problems.
