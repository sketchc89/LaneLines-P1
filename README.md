#**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm. This project's purpose was to detect lane lines in images using Python and OpenCV.  

--- 

#Files

* [writeup_template.md](writeup_template.md) contains a writeup of the project. It describes the project, how it works, its current limitations, and potential future paths.
* [P1.ipynb](P1.ipynb) contains all of the code written for this project. The pipeline is broken into individual cells for explanatory purposes before being combined to be run on the videos.
* [White](white.mp4), [Yellow](yellow.mp4), and [Extra](extra.mp4) are the video files generated from the lane finding program.
* [The results folder](results/) contains images showing steps along the pipeline. This is referenced in the writeup.
* [The canny folder](canny/) contains a group of images generated for selecting the optimal threshold for the Canny Edge detector
* [The masks folder](masks) contains images of the yellow, white, and combined masks used for the challenge.


