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

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I blurred the image in graysclae using gaussian_blur function with kernel size of 5. Then I applied Canny algorithm for edge extraction. Then I applied a interest of region mask onto the output image from Canny algorithm and leveraged Hough transformation for line detection. In order to gain the best combination of parameters for these two steps, I did parameter engineering by try out every combination of the parameters and figure out which has the best performance. Finally, I used weight_image function to put the lines onto original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by grouping every line by their slope - put lines having similar slope into the same group. For every group, draw a line cross the interest of region using the slope of the group.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![original image][test_images/solidWhiteCurve.jpg]
![processed image][test_images_output/solidWhiteCurve.jpg]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there are multiple parrellel lines are detected as lanes. In this situation, my draw_functions would choose one of them to draw the line. However, the chosen one might not be a segment of the lane, instead, it might be an environmental object like the shadow of trees.

Another shortcoming could be only straight line is supported by draw_lines function now. If the vehicle is driving on a curved road or making a sharp turn. Only tangent line will be drew on the image. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to apply more filters to remove environmental objects. 

Another potential improvement could be to use extrapolation to draw lines. Currently, I assume the line is straight and draw a straight line for every slope group. If it's possible to make sure that detected segments are mainly from the lane with more filters applied, we can use extrapolation to connect segments. 



