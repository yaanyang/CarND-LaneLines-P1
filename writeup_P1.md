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

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps:
(1) I converted the images to HSL color sapce, then I used 2 filters to detect white and yellow colors and generated masks.
Both masks were combined and coated with original image.

(2) I applied a Gaussian Blurring of the resulting image from (1) with kernel_size = 15.

(3) The blurred image was applied with Canny trasnform to detect edges of lines. The low/high tresholds were selected as 50/150.

(4) Then I applied a mask with vertices that only focus on the lane lines and turned all outside as black color.

(5) Then the masked image was applied with Hough's transform to detect lines with rho = 1, theta = np.pi/180, threshold = 15, min_line_len = 20, max_line_gap = 300.

(6) Finally, the resulting image was overlapped with original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by introducing average_lines() function.

Inside average_lines() function, I first separated all lines generated from Hough transform into left or right lines by their slopes (negative=left and positive=right). Then each line's slope, intercept and length were calculated and appended to corresponding list.

Then, all lines were averaged by their "weights" (lengths) by using dot product for left and right lanes. The outputs of average_lines() were avg_(slope, intercept) of each lane.

Before ploting in draw_lines() function, I used another function slope_intercept_to_points() to turn slope/intercept of a line into 2 extreme points which is the same as our interest region. 

Finally, each of left and right lanes was drawn onto original image and outputed as result.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when lane line is curved. My program can only fit fairly straight line. 

Another shortcoming could be when car is in sharp turn, also when car is going upward or downward. The interest region would need to be adjusted accordingly.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use polynominal lines to fit curved lane line.

Another potential improvement could be to detect end of vision height and adjust interest region accordingly.
