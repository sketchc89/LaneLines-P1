#**Finding Lane Lines on the Road** 

##Writeup 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image0]: ./results/image.jpg "Grayscale"[image1]: ./results/grayscale.jpg "Grayscale"
[image2]: ./results/blur.jpg "Gaussian Blur"
[image3]: ./results/canny.jpg "Canny Edge Detection"
[image4]: ./results/roi.jpg "Region of Interest"
[image5]: ./results/dilate.jpg "Dilation"
[image6]: ./results/hough.jpg "Hough"
[image7]: ./results/overlaid.jpg "Overlaid"
[image8]: ./masks/two_masks.jpg "Masks"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of 7 steps. 
![alt text][Reference Image]
1. The image is converted to gray-scale. ![Grayscale][image1]
2. The gray-scale image is blurred with a Gaussian kernel to remove noise that interferes with Canny edge detection's gradient finding algorithm. ![Gaussian blur][image2]
3. Canny edge detection finds the highest gradient region . Thresholds were selected by performing Canny edge detection with a range of thresholds on multiple images and visually selecting the best thresholds. ![Canny edge][image3]
4. A trapezoidal region of interest is selected from the image for further processing ![ROI][image4]
5. The lines within the region of interest were dilated to increase the likelihood of a common line being found. ![Dilation][image5]
6. Hough tranform is used to finds lines within the region of interest. The lines are extended to the horizon and the bottom of the image ![Dilation][image6]
7. A red line is drawn over the lines and blue trapezoid is overlaid on the lane region. ![Overlay][image7]

In order to draw a single line along the lane lines I modified the draw_line function. I created a function that determined the intersection point between any two vectors from two points on each vector. For each line the intersection of the line and a vector defining the horizon and the bottom of the image was calculated. The lines were separated into lines on the left and right side of the RoI by determining whether the slope of the line was positive (right) or negative (left). The average position of the points for the left and right groups are used when drawing the final line segment over the lane lines.

For the challenge I added a couple more steps. The brightness of one patch of road saturated the camera. In order to account for this I separated out the yellow and white lane lines by detecting only parts of the image with a specific HSV values. I then darkened the image to reduce the likelihood that the camera would be saturated. I also changed the ROI to account for the change in viewing angle. The camera was detecting the front of the car in this video. The following image is a mask of the yellow and white sections used on the challenge ![Masks][image8]

If you'd like to include images to show how the pipeline works, here is how to include an image: 


###2. Identify potential shortcomings with your current pipeline

The following are shortcomings of the current pipeline.
1. The current pipeline is not very robust to changes in brightness. It's able to compensate for changes in brightness for parts of the challenge, but the algorithm is very tuned to this specific example.
2. The current pipeline is not robust to curving roads. The interstate highway system in the U.S. is composed of segment of large straight sections and curving sections. The lanes identified by the pipeline diverge from the curved lines and mean the lane lines are accurate at a shorter distance or when the radius of curvature of the road is large or infinite (straight).
3. The current pipeline assumes the camera is calibrated correctly and doesn't attempt to correct distortions.
4. The current pipeline assumes that the horizon is at a fixed height. This means it's likely not robust to changes in viewing angle from camera tilt or going uphill or downhill.
5. The current pipeline can accidentally detect long straight lines that are not lane lines. The concrete highway barrier in the challenge is an example of a long straight object that is erroneously detected as a lane line.
6. The current pipeline is not robust to inclement weather like heavy rain, snow, or leaves that affect the visibility of the road.

###3. Suggest possible improvements to your pipeline

The following are suggested improvements to  improve on the shortcomings noted above. Each numbered item corresponds to the same numbered shortcoming above.
1. A possible improvement to solve problems with shading is to calculate a color transform to normalize the image.
2. A possible improvement to adjust to curving roads is to try to fit a polynomial line to the lane lines rather than assuming that the lanes are straight. A line fit could be used to find the point at which the lanes deviate from straight which would make the job of searching for the curved lines simpler.
3. A possible improvement would be to calculate the distortion of the camera by using a calibration matrix. Once the distortion is known, it could be removed from the images when finding lane lines.
4. A possible improvement would be to automatically calculate the horizon by using vanishing points of the lane lines. This could be done by calculating the point at which the lane lines cross one another.
5. A possible improvement to prevent the stray detection of lane lines is to constrain the detection of a lane line to the ground plane. In order to do this the ground plane would have to be separated from other planes such as walls. The color and texture of the surrounding area (concrete) could also be used to detect areas where lane lines are more likely to be detected.
6. Inclement weather is a more difficult problem than those identified above because visual systems of human drivers are similarly affected by these conditions. Further research is needed to understand how to tackle this problem with vision. A potential idea to investigate is using other driver's brake lights and snow tracks created by other drivers since people usually abandon covered lane lines and use these when driving in heavy snow. 
