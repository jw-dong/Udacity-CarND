# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_results/grayscale.png "Grayscale"
[image2]: ./test_results/gaussian.png "Gaussian"
[image3]: ./test_results/canny.png "Canny"
[image4]: ./test_results/hough.png "Hough"
[image5]: ./test_results/findlanes.png "Findlanes"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The code is in the "Pipeline for finding lanes" section, which consisted of 5 steps. 

1. I created a copy of the image and converted it to grayscale.

![Grayscale][image1]

2. I applied a gaussian smoothing to the grayscale image with a kernal size of 5.

![Gaussian][image2]

3. The Canny function was applied to the guassian smoothed image to fine the edges within the defined low and high thresholds. 

![Canny][image3]

4. A mask was created to only look for the lines in the region of interests. 

5. Hough transformation was applied to the edge image with the mask of region of interests. 

![Hough][image4]

6. In order to draw a single line on the left and right lanes, I created an extrapolate_lanes() function to seperate left lanes from the right lanes using the slopes calculated from the lines identified from the Hough transformation. To remove some noise and lines from shadows, I only looked for the lines with certain slopes. Once the slopes were calculated, the line euqitons y = m*x + b were defined for both the left and right lanes.

![Findlanes][image5]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming of the code would be that it cannot remove the lines created by shadows on the roads or different colors of road that caused by patches.  



### 3. Suggest possible improvements to your pipeline

A possible improvement that is to only look for lines with certain slopes, which was added to the code. 
