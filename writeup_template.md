# **Finding Lane Lines on the Road** 

---

### Overview

The goal of this project is to create a pipeline for detecting lanelines in an image. Using the test images provided by Udacity to achieve satisfactory results. After that we can try the pipeline to video files to see the result. 

---

### Reflection

### 1. Pipeline description.

My pipeline consists of 6 steps. Initially there were only 5 steps as I've skipped the color mask. The 2 videos ware performing good, but the bump on the challenge scattered the lines all over the place. 
So now the steps are: 
1. I apply a mask to filter the white and yellow colors of the image. Fortunately, lowering the RGB values covers the white and yellow range. 
2. I grayscale the masked image
3. Then apply a gaussian blur
4. Apply the canny function. I've discovered higher values give better results. 
5. Then, I apply a mask to get only the image of interest. I've decided to go down 60% of the Y axe and then make a trapezoid where the top line is 6% of the x axe. 
6. I then apply the hough lines and merge the result with the original image. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first making 2 lists separating the lines based on positive and negative slope (left and right). I ignore slopes lower than 0.2 (almost horizontal lines) in each direction. I then use a function process_line that takes the list and use np.mean to make an average of all the lines. 
Since a line can be represented as a function I then find the slope and intercept of the line. Using the same function (y = mx + b) I calculate the start point of the line at again 60% down of the y axe and the end point of the line at the bottom of the image. The process_line function is called 2 times for left and right line respectively. 


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when you have an arrow or another sign on the lane. 
There is a problem with the challenge video where sometimes the left and right lines cross. 
Another problem is that even with the color mask on the challenge video when the car gets to the bump, the lines go all over the place. 


### 3. Suggest possible improvements to your pipeline

If you store the result of the average line for each image in a sequence of 10 or more images, then the scattering and crossing of the lines can be lowered or overcome entirely. Although, even when the two line cross if you use the middle for the steering, the car would still steer in the right direction. 

If the crossing of the lines is a problem for the steering, it could be easily detected by comparing the Xs of the top point of the right and left line. 

The color mask can be made more precise, but since in my current country (Bulgaria) the lane line marks are missing most of the time I would work on not relying on them at all. 