# **Project: Finding Lane Lines on the Road**
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

The goal of this project ia to make a pipeline that finds lane lines on the road.
Therefore some test images in the subfolder `test_images` and two test videos (`test_videos/solidYellowLeft.mp4` and `test_videos/solidWhiteRight.mp4`) are provided by Udacity.
There is also one optional more difficult test video `test_videos/challenge.mp4`.
The image below shows the expected output of the project.

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

---

**Steps how I implemented this project**

## 1. Extend the Helper Functions

`read_img` and `write_img` were added in order to outsource image IO handling.
`write_img` also converts the image before saving it to disk, if it is a color image, from RGB (matplotlib) to BGR (OpenCV).

`ensure_color` takes a gray-scale or color image as input and always returns a color image with three channels.

`hough_lines` was split into `hough_lines` and `hough_lines_img`.
`hough_lines` returns the detected lines to the caller, where `hough_lines_img` returns an image with the lines drawn into that image, as the behavior of the original function was.

All other helper function were left as they were provided by Udacity.

## 2. Handling straights

Additionally there were three more functions added to handle straights.

`get_straight_params_from_2points` calculates the slop `m` of a straight through two given points and the y-axis intercept `t`.

`get_straight_params_from_line` takes a hough-line as input and also returns `(m,t)` of the straight.

`get_x_for_y_equals` calculates the `x`-coordinate of the interception point of the lines defined by `straight_params` [`(m,t)` which define the line `y=m*x+t`] and some horizontal line defined by `y=<some value>`.

## 3. Defining the pipeline

The `detectLaneLines` pipeline is defined in a separate function named equally.
It consists of the following nine steps:

1. convert the image to gray-scale
2. smooth high frequent noise with a gaussian blur
3. apply a canny edge detection
4. select a region of interest
    ToDo: include image
5. get lines from hough transformation
6. filter lines
    * two lists should be created: `left_lines` and `right_lines`
    * assuming the car on which the camera images are taken from is in a lane, there should be one lane pointing near to the bottom left corner and one lane pointing near to the bottom right corner of the image
    * therefore at the bottom of the image there are two areas defined (left and right)
    * lines which point into the left area are considered to be part of a left lane line and are added to the `left_lines` list
    * lines which point into the right area are considered to be part of a right lane line and are added to the `right_lines` list
7. building an average of left and right lane lines
    * assuming we are on a straight road
    * all lines on one lane line (left or right) should point to approximately to the same start and end point (bottom and top fo the image)
    * all starting points have a `y`-coordinate of `bottom` = bottom of ROI
    * an average over the `x`-coordinates is calculated
    * all end points have a `y`-coordinate of `top` = top of ROI
    * an average over the `x`-coordinates is calculated
    * one left and one right lane line is added to the lane lines list
8. draw lane lines into a clear image
9. add the original image with the drawn lane lines with the `weighted_img` function

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.

To complete the project, two files will be submitted: a file containing project code and a file containing a brief write up explaining your solution. We have included template files to be used both for the [code](https://github.com/udacity/CarND-LaneLines-P1/blob/master/P1.ipynb) and the [writeup](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md).The code file is called P1.ipynb and the writeup template is writeup_template.md

To meet specifications in the project, take a look at the requirements in the [project rubric](https://review.udacity.com/#!/rubrics/322/view)


Creating a Great Writeup
---
For this project, a great writeup should provide a detailed response to the "Reflection" section of the [project rubric](https://review.udacity.com/#!/rubrics/322/view). There are three parts to the reflection:

1. Describe the pipeline

2. Identify any shortcomings

3. Suggest possible improvements

We encourage using images in your writeup to demonstrate how your pipeline works.

All that said, please be concise!  We're not looking for you to write a book here: just a brief description.

You're not required to use markdown for your writeup.  If you use another method please just submit a pdf of your writeup. Here is a link to a [writeup template file](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md).


The Project
---

## If you have already installed the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) you should be good to go!   If not, you should install the starter kit to get started on this project. ##

**Step 1:** Set up the [CarND Term1 Starter Kit](https://classroom.udacity.com/nanodegrees/nd013/parts/fbf77062-5703-404e-b60c-95b78b2f3f9e/modules/83ec35ee-1e02-48a5-bdb7-d244bd47c2dc/lessons/8c82408b-a217-4d09-b81d-1bda4c6380ef/concepts/4f1870e0-3849-43e4-b670-12e6f2d4b7a7) if you haven't already.

**Step 2:** Open the code in a Jupyter Notebook

You will complete the project code in a Jupyter notebook.  If you are unfamiliar with Jupyter Notebooks, check out <A HREF="https://www.packtpub.com/books/content/basics-jupyter-notebook-and-python" target="_blank">Cyrille Rossant's Basics of Jupyter Notebook and Python</A> to get started.

Jupyter is an Ipython notebook where you can run blocks of code and see results interactively.  All the code for this project is contained in a Jupyter notebook. To start Jupyter in your browser, use terminal to navigate to your project directory and then run the following command at the terminal prompt (be sure you've activated your Python 3 carnd-term1 environment as described in the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) installation instructions!):

`> jupyter notebook`

A browser window will appear showing the contents of the current directory.  Click on the file called "P1.ipynb".  Another browser window will appear displaying the notebook.  Follow the instructions in the notebook to complete the project.

**Step 3:** Complete the project and submit both the Ipython notebook and the project writeup

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

